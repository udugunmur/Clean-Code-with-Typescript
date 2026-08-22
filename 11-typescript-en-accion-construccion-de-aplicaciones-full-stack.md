# Parte 3: TypeScript Avanzado y Aplicaciones en el Mundo Real

## Capítulo 11: TypeScript en Acción: Construcción de Aplicaciones Full Stack

### Sección: Introducción

En el capítulo anterior, establecimos las bases para proyectos escalables de TypeScript, pasando de definir una especificación de producto a configurar el monorepo de nuestro portal DevJobs utilizando Nx, además de automatizar comprobaciones y aplicar estándares de código con Git hooks.

Con la estructura tanto del backend como del frontend lista, este capítulo se centra en dar vida al proyecto. Comenzaremos con un backend en NestJS, definiendo APIs con seguridad de tipos (*type-safe*) y validación, y luego pasaremos al frontend en React, consumiendo esas APIs mediante Objetos de Transferencia de Datos (*Data Transfer Objects* o DTOs) compartidos. A lo largo del camino, exploraremos cómo TypeScript fortalece la colaboración reduciendo los errores en tiempo de ejecución y asegurando la coherencia entre el cliente y el servidor.

Al final de este capítulo, tendrás una aplicación *full stack* de nivel de producción y completamente tipada que demuestra el verdadero poder de TypeScript en el desarrollo web moderno.

En este capítulo, cubriremos los siguientes temas principales:

- TypeScript en el lado del servidor con Node.js
- TypeScript en el lado del cliente con React

---

### Sección: Requisitos técnicos

Para seguir este capítulo, deberás haber completado la configuración del proyecto del Capítulo 10. Puedes descargar el proyecto de ejemplo y el código de este libro siguiendo las instrucciones en la sección *Download the example code files* en el Prefacio de este libro.

Los archivos de código de este capítulo están incluidos en el paquete de código descargable.

---

### Sección 1: TypeScript en el lado del servidor con Node.js

Una de las contribuciones más significativas de Node.js fue brindar a los desarrolladores la capacidad de ejecutar JavaScript fuera del navegador, abriendo la puerta al desarrollo *full stack* donde los equipos construyen aplicaciones cliente y servidor utilizando el mismo lenguaje. Con TypeScript, esta sinergia se fortalece gracias a la seguridad de tipos, mejores herramientas y una base más confiable para la colaboración integral.

En esta sección, nos enfocaremos en desarrollar el backend para nuestra aplicación DevJobs utilizando TypeScript y NestJS. Aplicaremos el enfoque *contract-first* (contrato primero), definiendo esquemas y DTOs compartidos que estandarizan cómo se intercambia la información entre el cliente y el servidor, sirviendo como la única fuente de verdad (*single source of truth*).

#### Implementación del enfoque contract-first

El enfoque *contract-first* se centra en definir primero los esquemas esperados para nuestra API (tipos de datos de solicitud y respuesta, junto con los métodos disponibles) en estrecha colaboración entre desarrolladores frontend y backend. Este acuerdo se captura formalmente utilizando una especificación OpenAPI (Swagger).

Beneficios clave:
- **Fuente compartida de verdad**: Elimina la ambigüedad en la comunicación entre equipos.
- **Desarrollo en paralelo**: El equipo frontend puede simular (*mockear*) respuestas y construir interfaces sin esperar la implementación final del backend.
- **Compatibilidad hacia atrás**: Cualquier refactorización interna del backend debe seguir cumpliendo con el contrato pactado.
- **Seguridad de tipos integral**: Generación automática de tipos TypeScript a partir de la documentación OpenAPI para clientes y servidores.

##### Paso 1: Creación del archivo `openapi.yml`

En el directorio del proyecto `devjobs-backend`, crea un archivo `openapi.yml`.

##### Paso 2: Definición de la especificación de la API

Añadimos la definición de la API en `apps/devjobs-backend/src/openapi.yml`:

```yaml
openapi: 3.0.3 info: title: DevJobs API version: 1.0.0 description: API for the DevJobs platform servers: - url: http://localhost:3000 description: Local development server paths: /users: get: summary: List all users responses: '200': description: Array of users content: application/json: schema: type: array items: $ref: '#/components/schemas/User' post: summary: Register a new user requestBody: required: true content: application/json: schema: $ref: '#/components/schemas/CreateUser' responses: '201': description: Created user content: application/json: schema: $ref: '#/components/schemas/User'
```

Endpoints principales definidos en la especificación:
- `/users`: `GET` (listar usuarios), `POST` (registrar usuario)
- `/companies`: `GET` (listar empresas), `POST` (crear empresa)
- `/jobs`: `GET` (listar empleos), `POST` (crear oferta de empleo)

##### Paso 3: Instalación de Orval y configuración

Orval es un paquete que genera firmas de tipos y clientes a partir de documentación OpenAPI v3 o v2:

```bash
npm install --save-dev orval
```

Crea el archivo `orval.config.ts` en la raíz del proyecto:

```typescript
export default { devjobs: { input: './apps/devjobs-backend/src/openapi.yml', output: { schemas: './libs/api-types/model', target: './libs/api-types', }, }, };
```

##### Paso 4: Generación de tipos con Orval

Ejecuta el generador en tu terminal:

```bash
npx orval
```

Esto generará las interfaces y modelos de datos en `libs/api-types/model`.

#### Estructura modular de NestJS

En NestJS, la arquitectura se divide en:
- **Modules (`app.module.ts`)**: Contenedores que agrupan controladores, servicios y proveedores relacionados.
- **Controllers (`app.controller.ts`)**: Definen las rutas y manejan las solicitudes entrantes y respuestas HTTP.
- **Services (`app.service.ts`)**: Contienen la lógica de negocio y el acceso a datos.

##### Adición de módulos de funcionalidad

Estructura para la funcionalidad de usuarios (`Users`):

```text
src/ ├── app.controller.ts ├── app.module.ts ├── app.service.ts ├── main.ts └── users/ ├── users.module.ts ├── users.controller.ts └── users.service.ts
```

Registro del módulo en `app.module.ts`:

```typescript
// app.module.ts import { Module } from '@nestjs/common'; import { AppController } from './app.controller'; import { AppService } from './app.service'; import { UsersModule } from './users/users.module'; @Module({ imports: [UsersModule], // registering feature module controllers: [AppController], providers: [AppService], }) export class AppModule {}
```

Comparativa de enrutamiento entre Express y NestJS:

En Express:
```typescript
app.get('/jobs', (req, res) => { ... });
```

En NestJS:
```typescript
@Controller('jobs') export class JobsController { constructor(private readonly jobsService: JobsService) {} @Get() findAll() { return this.jobsService.findAll(); } }
```

#### Módulo de Usuarios (`UsersModule`)

Generación de componentes con esquemáticos de NestJS:

```bash
npx nx g @nx/nest:module --path=apps/devjobs-backend/src/app/users/users npx nx g @nx/nest:controller --path=apps/devjobs-backend/src/app/users/users npx nx g @nx/nest:service --path=apps/devjobs-backend/src/app/users/users
```

Clase base `UsersService`:

```typescript
import { Injectable } from '@nestjs/common'; @Injectable() export class UsersService {}
```

Controlador `users.controller.ts`:

```typescript
import { Body, Controller, Post } from '@nestjs/common'; import { UsersService } from './users.service'; @Controller('users') export class UsersController { constructor(private readonly usersService: UsersService) {} @Post() register(@Body() body: any) { return this.usersService.register(body); }}
```

##### Lógica de registro y hashing con bcrypt

Instalación de bcrypt y sus tipos:

```bash
pnpm install bcrypt -w pnpm install --save-dev @types/bcrypt -w
```

Implementación de `register` en `UsersService`:

```typescript
async register(user: CreateUser): Promise<UserResponse> { const hashedPassword = await bcrypt.hash(user.password, 10); const newUser = { id: randomUUID(), name: user.name, email: user.email, role: user.role, companyId: user.companyId, password: hashedPassword, }; this.users.push(newUser); const { password, ...userWithoutPassword } = newUser; return userWithoutPassword; }
```

Petición de prueba para registrar un usuario:

```json
{ "name": "John Doe", "email": "john@example.com", "password": "securePassword123", "role": "user", "companyId": "company-uuid-123" }
```

Respuesta esperada:

```json
{ "id": "e9e330ae-31bb-4d1c-b202-f7afca956772", "name": "John Doe", "email": "john@example.com", "role": "user", "companyId": "company-uuid-123" }
```

Método de búsqueda por correo electrónico:

```typescript
findByEmail(email: string): User | undefined { return this.users.find((user) => user.email === email); }
```

#### Autenticación y Autorización con JWT

##### Paso 1: Instalación de dependencias de autenticación

```bash
pnpm install @nestjs/jwt @nestjs/passport passport passport-jwt bcrypt pnpm install -D @types/passport-jwt
```

##### Paso 2: Generación del módulo Auth

```bash
npx nx g @nx/nest:module --path=apps/devjobs-backend/src/app/auth/auth npx nx g @nx/nest:controller --path=apps/devjobs-backend/src/app/auth/auth npx nx g @nx/nest:service --path=apps/devjobs-backend/src/app/auth/auth
```

##### Paso 3 y 4: Configuración de `AuthModule`

```typescript
import { Module } from '@nestjs/common'; import {JwtModule} from '@nestjs/jwt' import { PassportModule } from '@nestjs/passport'; import { UsersModule } from '../users/users.module'; import { AuthController } from './auth.controller'; import { AuthService } from './auth.service';
```

```typescript
@Module({ imports: [ UsersModule, PassportModule, JwtModule.register({ secret: 'super-secret-key', // replace with env var in production signOptions: { expiresIn: '1h' }, }), ], controllers: [AuthController], providers: [AuthService, JwtStrategy], exports: [AuthService], })
```

> **Nota de producción**: Para sistemas distribuidos o de alta seguridad, se recomienda utilizar firmas asimétricas (RS256) con par de claves pública/privada:

```typescript
JwtModule.register({ privateKey: process.env.JWT_PRIVATE_KEY, publicKey: process.env.JWT_PUBLIC_KEY, signOptions: { algorithm: 'RS256', expiresIn: '1h', }, });
```

##### Paso 5: Lógica de autenticación en `AuthService`

```typescript
import { Injectable, UnauthorizedException } from '@nestjs/common'; import { JwtService } from '@nestjs/jwt'; import * as bcrypt from 'bcrypt'; import { UsersService } from '../users/users.service';
```

```typescript
@Injectable() export class AuthService { constructor( private usersService: UsersService, private jwtService: JwtService, ) {} //...... }
```

```typescript
async validateUser(email: string, pass: string) { const user = await this.usersService.findByEmail(email); if (user && (await bcrypt.compare(pass, user.password))) { const { password, ...result } = user; return result; } return null; }
```

```typescript
async login(email: string, password: string) { const user = await this.validateUser(email, password); if (!user) { throw new UnauthorizedException('Invalid credentials'); } const payload = { sub: user.id, role: user.role }; return { access_token: this.jwtService.sign(payload), }; }
```

##### Paso 6: Endpoint `/auth/login` en `AuthController`

```typescript
import { Body, Controller, Post } from '@nestjs/common'; import { AuthService } from './auth.service'; @Controller('auth') export class AuthController { constructor(private readonly authService: AuthService) {} @Post('login') async login(@Body() body: { email: string; password: string }) { return this.authService.login(body.email, body.password); } }
```

##### Paso 7: Exportación de `UsersService` y prueba del flujo

Exporta `UsersService` en `UsersModule`:

```typescript
@Module({ providers: [UsersService], exports: [UsersService], // Add this line }) export class UsersModule {}
```

Registro mediante `curl`:

```bash
curl -X POST http://localhost:3000/users \ -H "Content-Type: application/json" \ -d '{ "name": "Test User", "email": "test@example.com", "password": "password123", "role": "user", "companyId": "demo-company" }'
```

Inicio de sesión mediante `curl`:

```bash
curl -X POST http://localhost:3000/auth/login \ -H "Content-Type: application/json" \ -d '{ "email": "test@example.com", "password": "password123" }'
```

Respuesta con token JWT:

```json
{ "access_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..." }
```

Carga útil decodificada (*payload*):

```json
{ "sub": "c6762198-c77d-4aab-a0b2-009ae1df94a7", "role": "user", "iat": 1759521269, "exp": 1759524869 }
```

Carga segura mediante variables de entorno en producción:

```typescript
JwtModule.registerAsync({ inject: [ConfigService], useFactory: (config: ConfigService) => ({ secret: config.get<string>('JWT_SECRET'), signOptions: { expiresIn: '1h' }, }), })
```

```text
JWT_SECRET=super-secret-key
```

#### Protección de rutas con JWT Guards

```typescript
import { Controller, Get, UseGuards, Request } from '@nestjs/common'; import { AuthGuard } from '@nestjs/passport'; @Controller('profile') export class ProfileController { @UseGuards(AuthGuard('jwt')) @Get() getProfile(@Request() req) { return req.user; // The decoded JWT payload } }
```

#### Persistencia basada en archivos JSON

##### Paso 1: Utilidad genérica `FileStorage`

Crea `apps/devjobs-backend/src/common/file-storage.ts`:

```typescript
import { existsSync, readFileSync, writeFileSync } from 'fs'; import { join } from 'path'; export class FileStorage<T> { private filePath: string; constructor(filename: string) { this.filePath = join(process.cwd(), 'data', filename); } private ensureFileExists() { if (!existsSync(this.filePath)) { writeFileSync(this.filePath, JSON.stringify([], null, 2)); } } read(): T[] { this.ensureFileExists(); const content = readFileSync(this.filePath, 'utf-8'); return JSON.parse(content) as T[]; } write(data: T[]) { writeFileSync(this.filePath, JSON.stringify(data, null, 2)); } }
```

Crea el directorio de almacenamiento:

```bash
mkdir apps/devjobs-backend/data
```

##### Paso 2: Actualización de `UsersService` con persistencia

```typescript
import { Injectable } from '@nestjs/common'; import * as bcrypt from 'bcrypt'; import { randomUUID } from 'crypto'; import { FileStorage } from '../common/file-storage'; @Injectable() export class UsersService { private storage = new FileStorage<any>('users.json'); async register(user: any) { const users = this.storage.read(); const hashedPassword = await bcrypt.hash(user.password, 10); const newUser = { id: randomUUID(), name: user.name, email: user.email, password: hashedPassword, role: user.role, companyId: user.companyId, }; users.push(newUser); this.storage.write(users); const { password, ...userWithoutPassword } = newUser; return userWithoutPassword; } findByEmail(email: string) { const users = this.storage.read(); return users.find((u) => u.email === email); } }
```

#### Módulo de Empresas (`CompaniesModule`)

##### Paso 1: Generación del módulo

```bash
npx nx g @nx/nest:module --path=apps/devjobs-backend/src/app/companies/companies npx nx g @nx/nest:controller --path=apps/devjobs-backend/src/app/companies/companies npx nx g @nx/nest:service --path=apps/devjobs-backend/src/app/companies/companies
```

##### Paso 2: Definición de `CompaniesService`

```typescript
import { Injectable } from '@nestjs/common'; import { randomUUID } from 'crypto'; import { FileStorage } from '../common/file-storage'; export interface Company { id: string; name: string; description: string; location: string; } @Injectable() export class CompaniesService { private storage = new FileStorage<Company>('companies.json'); findAll(): Company[] { return this.storage.read(); } create(company: Omit<Company, 'id'>): Company { const companies = this.storage.read(); const newCompany = { id: randomUUID(), ...company }; companies.push(newCompany); this.storage.write(companies); return newCompany; } }
```

##### Paso 3: Definición de `CompaniesController`

```typescript
import { Controller, Get, Post, Body, UseGuards, Request } from '@nestjs/common'; import { CompaniesService } from './companies.service'; import { AuthGuard } from '@nestjs/passport'; @Controller('companies') export class CompaniesController { constructor(private readonly companiesService: CompaniesService) {} @Get() findAll() { // Public route: returns all company profiles return this.companiesService.findAll(); } @UseGuards(AuthGuard('jwt')) @Post() create(@Body() body, @Request() req) { // Protected route: only users with 'company' role can create companies if (req.user.role !== 'company') { return { error: 'Only company accounts can create companies.' }; } return this.companiesService.create(body); } }
```

#### Módulo de Empleos (`JobsModule`)

##### Paso 1: Definición de `JobsService`

```typescript
import { Injectable } from "@nestjs/common"; import { randomUUID } from "crypto"; import { FileStorage } from "../common/file-storage"; import { Job } from "../../../../../libs/api-types/model"; @Injectable() export class JobsService { private storage = new FileStorage<Job>("jobs.json"); findAll(filters?: Partial<Job>): Job[] { let jobs = this.storage.read(); if (filters) { jobs = jobs.filter((job) => Object.entries(filters).every(([key, val]) => Val ? job[key as keyof Job]?.toString().includes(val.toString()) : true ) ); } return jobs; } create(job: Omit<Job, "id">): Job { const jobs = this.storage.read(); const newJob = { id: randomUUID(), ...job }; jobs.push(newJob); this.storage.write(jobs); return newJob; } }
```

##### Paso 2: Definición de `JobsController`

```typescript
import { Controller, Get, Post, Body, Query, UseGuards, Request } from '@nestjs/common'; import { JobsService } from './jobs.service'; import { AuthGuard } from '@nestjs/passport'; @Controller('jobs') export class JobsController { constructor(private readonly jobsService: JobsService) {} @Get() findAll(@Query() query) { return this.jobsService.findAll(query); } @UseGuards(AuthGuard('jwt')) @Post() create(@Body() body, @Request() req) { if (req.user.role !== 'company') { return { error: 'Only company users can post jobs.' }; } return this.jobsService.create({ ...body, companyId: req.user.sub }); } }
```

#### Pruebas del flujo completo del backend

1. Registrar un reclutador con rol `company`:
```bash
curl --location --request POST 'http://localhost:3000/api/users' \ --header 'Content-Type: application/json' \ --data-raw '{ "name": "Jane Doe Recruiter", "email": "janedoe@devjobs.com", "password": "password123", "role": "company", "companyId": "ffdba55c-78c2-44e3-9665-44d0582c1051" }'
```

2. Iniciar sesión y obtener el token JWT:
```bash
curl --location --request POST 'http://localhost:3000/api/auth/login' \ --header 'Content-Type: application/json' \ --data-raw '{ "email": "janedoe@devjobs.com", "password": "password123" }'
```

3. Usar el token en las cabeceras de autorización:
```http
Authorization: Bearer <token>
```

4. Crear una empresa:
```bash
curl --location --request POST 'http://localhost:3000/api/companies' \ --header 'Authorization: Bearer <add your token here... >' \ --header 'Content-Type: application/json' \ --data-raw '{ "name": "jane-doe-recruitment-company", "website": "www.janedoerecruitement.com" }'
```

5. Publicar un empleo:
```bash
curl --location --request POST 'http://localhost:3000/api/jobs' --header 'Authorization: Bearer <add your token here...>' --header 'Content-Type: application/json' --data-raw ' { "title": "Senior TypeScript Engineer", "companyId": "d9d5927a-b327-4b09-80d3-ade23c4e547b", "location": "Netherlands", "description": "We'''re seeking an exceptional Senior TypeScript Engineer to join our team in the Netherlands. As a Senior TypeScript Engineer, you will be responsible for developing, maintaining, and extending our microservices platform, handling backend systems, and collaborating on API-driven applications. You will work closely with our engineering team to build scalable, efficient, and reliable software solutions.\n\nKey Responsibilities:\n Develop high-quality software solutions using TypeScript,....." } '
```

6. Consultar empleos con filtros:
```bash
curl --location --request GET 'http://localhost:3000/api/jobs?tech=typescript&location=Netherlands' \ --data-raw ''
```

---

### Sección 2: TypeScript en el lado del cliente con React

Con el backend en funcionamiento, construimos la aplicación frontend en React que consumirá los endpoints utilizando los tipos y hooks generados automáticamente por Orval y React Query (TanStack Query).

#### Instalación de dependencias frontend

```bash
pnpm install react-router-dom @tanstack/react-query axios jwt-decode pnpm install -D @types/react-router-dom
```

#### Creación de la instancia del cliente Axios

Crea el directorio y el archivo `libs/api-client/src/axios.ts`:

```bash
mkdir -p libs/api-client/src
```

```typescript
import axios, { AxiosRequestConfig } from 'axios'; const instance = axios.create({ baseURL: '/api', }); // Add auth token to requests instance.interceptors.request.use((config) => { const token = localStorage.getItem('access_token'); if (token) { config.headers.Authorization = `Bearer ${token}`; } return config; }); export const api = <T>(config: AxiosRequestConfig): Promise<T> => { return instance.request<T>(config).then((res) => res.data); };
```

#### Configuración de Orval para generar hooks de React Query

Actualiza `orval.config.ts`:

```typescript
export default { devjobs: { input: './apps/devjobs-backend/src/openapi.yml', output: { target: 'libs/api-client/src/generated', // folder for generated files client: 'react-query', mode: 'tags', // separate file per tag override: { mutator: { path: './libs/api-client/src/axios.ts', name: 'api', }, }, prettier: true, index: true, clean: true, }, hooks: { afterAllFilesWrite: [ () => { console.log(' DevJobs API client generated successfully'); }, ], }, }, };
```

Estructura de hook generado en `libs/api-client/src/generated/authentication.ts`:

```typescript
//... The rest of our code is above: export const usePostAuthLogin = <TError = ErrorResponse, TContext = unknown>(options?: { mutation?:UseMutationOptions<Awaited<ReturnType<typeof postAuthLogin>>, TError,{data: LoginRequest}, TContext>, } , queryClient?: QueryClient): UseMutationResult< Awaited<ReturnType<typeof postAuthLogin>>, TError, {data: LoginRequest}, TContext > => { const mutationOptions = getPostAuthLoginMutationOptions(options); return useMutation(mutationOptions, queryClient); }
```

#### Configuración del proveedor de React Query

Ejecuta Orval:

```bash
npx orval
```

Actualiza `apps/devjobs-frontend/src/main.tsx`:

```typescript
import { StrictMode } from "react"; import { BrowserRouter } from "react-router-dom"; import * as ReactDOM from "react-dom/client"; import { QueryClient, QueryClientProvider } from "@tanstack/react-query"; import App from "./app/app"; const queryClient = new QueryClient({ defaultOptions: { queries: { refetchOnWindowFocus: false, retry: 1, }, }, }); const root = ReactDOM.createRoot( document.getElementById("root") as HTMLElement ); root.render( ( <StrictMode> <QueryClientProvider client={queryClient}> <BrowserRouter> <App /> </BrowserRouter> </QueryClientProvider> </StrictMode> ) as React.ReactNode );
```

#### Construcción del flujo de autenticación con React Context

Crea `apps/devjobs-frontend/src/contexts/AuthContext.tsx`:

```bash
mkdir -p apps/devjobs-frontend/src/contexts
```

```typescript
import { createContext, useContext, useState, useEffect, ReactNode } from 'react'; import jwtDecode from 'jwt-decode'; interface DecodedToken { sub: string; role: string; exp?: number; } interface AuthContextType { token: string | null; user: { userId: string; role: string } | null; login: (token: string) => void; logout: () => void; isAuthenticated: boolean; }
```

```typescript
const AuthContext = createContext<AuthContextType | undefined>(undefined);
```

```typescript
export function AuthProvider({ children }: { children: ReactNode }) { const [token, setToken] = useState<string | null>(null); const [user, setUser] = useState<{ userId: string; role: string } | null>(null); // Load token from localStorage on mount useEffect(() => { const storedToken = localStorage.getItem('access_token'); if (storedToken) { setToken(storedToken); } }, []); }
```

```typescript
useEffect(() => { if (!token) { setUser(null); return; } try { const decoded = jwtDecode<DecodedToken>(token); // Optional: check if token is expired if (decoded.exp && decoded.exp * 1000 < Date.now()) { console.warn('JWT expired'); logout(); return; } setUser({ userId: decoded.sub, role: decoded.role }); } catch (error) { console.error('Invalid token:', error); logout(); } }, [token]); const login = (newToken: string) => { localStorage.setItem('access_token', newToken); setToken(newToken); }; const logout = () => { localStorage.removeItem('access_token'); setToken(null); setUser(null); };
```

```typescript
const login = useCallback((newToken: string) => { localStorage.setItem('access_token', newToken); setToken(newToken); }, []); const logout = useCallback(() => { localStorage.removeItem('access_token'); setToken(null); setUser(null); }, []);
```

```typescript
return ( <AuthContext.Provider value={{ token, user, login, logout, isAuthenticated: !!user && !!token, }} > {children} </AuthContext.Provider> );
```

```typescript
export const useAuth = () => { const context = useContext(AuthContext); if (!context) { throw new Error('useAuth must be used within an AuthProvider'); } return context; };
```

Envolver la aplicación con `AuthProvider` en `main.tsx`:

```typescript
// rest of you code above ... <AuthProvider> <BrowserRouter> <App /> </BrowserRouter> </AuthProvider> // rest of you code below ...
```

#### Construcción de la página de Login (`Login`)

##### Paso 1: Componente inicial

`apps/devjobs-frontend/src/pages/Login/index.tsx`:

```typescript
export function Login() { return <div>Login Component</div>; }
```

##### Paso 2: Integración en el enrutador (`app.tsx`)

```typescript
import { Route, Routes } from 'react-router-dom'; import { Login } from './Login'; export function App() { return ( <Routes> <Route path="/login" element={<Login />} /> </Routes> ); } export default App;
```

##### Paso 3: Estructura visual con TailwindCSS

```typescript
export function Login() { return ( <div className="min-h-screen flex items-center justify-center"> <div className="w-full max-w-sm"> <h1 className="text-xl font-semibold">Login</h1> </div> </div> ); }
```

##### Paso 4: Formulario y campos de entrada

```html
<form> <div className='flex flex-col space-y-1'> <label htmlFor='email' className='text-sm'> Email </label> <input id='email' type='email' className='border px-3 py-2 rounded' /> </div> <div className='flex flex-col space-y-1'> <label htmlFor='password' className='text-sm'> Password </label> <input id='password' type='password' className='border px-3 py-2 rounded' /> </div> <button type='submit' className='w-full border py-2 rounded'> Login </button> </form>
```

##### Paso 5: Control de estado con React

```typescript
import { useState } from 'react';
```

```typescript
export function Login() { const [email, setEmail] = useState(''); const [password, setPassword] = useState(''); // rest of the component }
```

```html
<input id='email' type='email' className='border px-3 py-2 rounded' onChange={(e) => setEmail(e.target.value)}/> <input id='password' type='password' className='border px-3 py-2 rounded' onChange={(e) => setPassword(e.target.value)}/>
```

##### Paso 6: Manejador de envío

```typescript
const handleSubmit = async (e: React.FormEvent) => { e.preventDefault(); console.log('Attempting login with email:', email, 'and password:', password); };
```

```html
<form onSubmit={handleSubmit} className="space-y-4">
```

Configuración de alias y rutas en `tsconfig.base.json` y `vite.config.ts`:

```json
{ "compilerOptions": { { "baseUrl": ".", "paths": { "@devjobs/api-client": ["libs/api-client/index.ts"], "@devjobs/api-client/*": ["libs/api-client/src/*"] } } }
```

```typescript
import { nxViteTsPaths } from '@nx/vite/plugins/nx-tsconfig-paths.plugin';
```

```typescript
export default defineConfig({ plugins: [react(), nxViteTsPaths()], });
```

##### Pasos 7 y 8: Llamada a la API de Login y almacenamiento del Token

```typescript
import { usePostAuthLogin } from '@devjobs/api-client';
```

```typescript
const loginMutation = usePostAuthLogin();
```

```typescript
import { useAuth } from '../../context/AuthContext';
```

```typescript
const { login } = useAuth(); const handleSubmit = async (e: React.FormEvent) => { e.preventDefault(); if (!email || !password) return; const response = await loginMutation.mutateAsync({ data: { email, password }, }); login(response.data.access_token); };
```

##### Paso 9: Visualización de mensajes de error

```html
{loginMutation.isError && ( <p className='text-sm'> Login failed. Please check your email and password. </p> )}
```

```typescript
const handleSubmit = async (e: React.FormEvent) => { try { e.preventDefault(); if (!email || !password) return; const response = await loginMutation.mutateAsync({ data: { email, password }, }); login(response.data.access_token); } catch (error) { // you can use your custom loggers here to handle the error in a better way console.error('Login failed:', error); // } };
```

##### Paso 10: Deshabilitar el botón durante la petición

```html
<button type="submit" disabled={loginMutation.isPending} > {loginMutation.isPending ? 'Logging in...' : 'Login'} </button>
```

Componente `Jobs` y redirección:

```typescript
// ch10/devjobs/apps/devjobs-frontend/src/app/Jobs/index.tsx export function Jobs() { return <div>Jobs Page</div>; }
```

```typescript
// export function App() { return ( <div> <Routes> <Route path='/login' element={<Login />} /> <Route path='/jobs' element={<Jobs />} /> </Routes> </div> ); }
```

```typescript
import { useNavigate } from 'react-router-dom';
```

```typescript
const navigate = useNavigate();
```

```typescript
const handleSubmit = async (e: React.FormEvent) => { try { // ... rest of your code navigate('/jobs') } catch (error) { // you can use your custom loggers here to handle the error in a better way console.error('Login failed:', error); // } };
```

#### Rutas protegidas (`ProtectedRoute`)

Crea `devjobs/apps/devjobs-frontend/src/app/ProtectedRoute.tsx`:

```typescript
import { Navigate } from 'react-router-dom'; import { useAuth } from '../context/AuthContext'; export function ProtectedRoute({ children }: { children: JSX.Element }) { const { isAuthenticated } = useAuth(); if (!isAuthenticated) { return <Navigate to="/login" replace />; } return children; }
```

Enrutador principal protegido en `app.tsx`:

```typescript
export function App() { return ( <Routes> <Route path="/login" element={<Login />} /> <Route path="/jobs" element={ <ProtectedRoute> <Jobs /> </ProtectedRoute> } /> <Route path="/" element={<Navigate to="/jobs" replace />} /> </Routes> ); }
```

#### Construcción de la página de Empleos (`Jobs`)

##### Paso 1: Contenedor principal

```typescript
export function Jobs() { return ( <div className="min-h-screen flex items-center justify-center"> <div className="w-full max-w-2xl"> <h1 className="text-xl font-semibold">Jobs</h1> </div> </div> ); }
```

##### Paso 2: Consulta con `useGetJobs`

```typescript
import { useGetJobs } from '@devjobs/api-client';
```

```typescript
const jobsQuery = useGetJobs();
```

##### Paso 3: Estados de carga y error

```typescript
if (jobsQuery.isLoading) { return <div className="p-4">Loading jobs...</div>; } if (jobsQuery.isError) { return <div className="p-4">Failed to load jobs.</div>; }
```

##### Paso 4: Renderizado de la lista de empleos

```typescript
const jobs = jobsQuery.data?.data ?? [];
```

```html
<ul className='space-y-2'> {jobs.map((job) => ( <li key={job.id} className='border rounded p-3'> <div className='font-medium'>{job.title}</div> <div className='text-sm text-gray-600'>{job.location}</div> <p className='text-sm text-gray-700 line-clamp-2'> {job.description} </p> </li> ))} </ul>
```

---

### Sección: Resumen

En este capítulo, aplicamos los conceptos esenciales de TypeScript para construir una aplicación *full stack* completa. Adoptamos el enfoque *contract-first* utilizando especificaciones OpenAPI como la única fuente de verdad entre el cliente y el servidor.

A partir de este contrato, generamos clientes tipados y hooks de React Query mediante Orval para alimentar nuestro frontend en React con total seguridad de tipos. En el backend, construimos una arquitectura modular con NestJS, incorporando autenticación con JWT, control de acceso basado en roles con guards y persistencia de datos.

En el próximo capítulo, profundizaremos en el diseño de arquitecturas robustas para **TypeScript en Sistemas en Evolución**.
