# Sophia Rest API Documentation

Version 2 of the SophiaAPIRest is structured around REST, HTTP, and JSON. API endpoint URLs are organized around resources, such as organitation or metodologies. It uses HTTP methods for indicating the action to take on a resource, and **HTTP status codes for expressing error states**. Resources are represented in JSON following a conventional schema.

# Access URL

The API is accessed using a base URL that is specific resource like this: `https://appweb.kaantera.com/api/2/<resource>`.

# Authentication

All requests to the API are authenticated by providing an User and Password to get an API-Token.

# Schema

All API requests and response bodies adhere to a common JSON format representing individual items, collections of items.

## Single Resources

Individual resources are represented by top level member named after the resource in the singular form. Below is a representation of a single **user**. This could be used in the body of a POST request and it's what would be returned in the body of a GET request.

```javascript
{
	"user":{
		"user": "celta",
		"pass": "12345",
		"company": "global s.a",
		"name": "Celta Campeche"
	}
}
```

## Collections

Collections of resources are represented by a top level member named after the resource in the plural form. Bellow is a representation of a collection of **users**.

```javascript
{
	"users":[
		{
			"user": "celta",
			"pass": "12345",
			"company": "global s.a",
			"name": "Celta Campeche"
		},
		{
			"user": "celta_trainer",
			"pass": "54321",
			"company": "global s.a",
			"name": "Celta Campeche"
		}
	]
}
```

# Errors

The API uses HTTP status codes to indicate an error has ocurred while processing a request. There are three main error status codes used by the API:

| Code | Description                                                                                                          |
| ---- | -------------------------------------------------------------------------------------------------------------------- |
| 403  | The request could not be authenticated or the authenticated user is not authorized to access the requested resource. |
| 404  | The requested resource does not exist.                                                                               |
| 422  | The request could not be processed, usually due to a missing or invalid parameter o json error.                      |

In the case of 422 errors the response will also include an error object with an explanation of fields that are missing or invalid. Here is an example:

```javascript
HTTP/1.1 422 Unprocessable Entity
{
	"errors":[
		{
			"description": "There isn't a JSON object in the request"
		},
		{
			"description": "Error in database connection"
		}
	]
}
```

[Changelog](Changelog.md)

# UsersV2

## Actions for users controller

|Verb  | URI                               | Action     | Route Name                      |Response Type                   |
|------|-----------------------------------|------------|---------------------------------|--------------------------------|
|GET   | `api/2/users`                     | list       | `UsersControllerV2#list()`      | [`ListWCursor<T>`](#responses) |
|GET   | `api/2/users/{id}`                | single     | `UsersControllerV2#single()`    | `<T>`                          |

Here, `<T>` equals to the `Type`, and in this particular case is `UserDomain` (defined [below](#type-definition))

### Type definition

```ts
interface UserDomain {
  id: number;
  empresa: string;
  header: string;
  pass: string;
  pin: string;
  phoneNumber: string;
  profile: int;
  profileStr: string;
  user: string;
  nombre: string;
  lastName: string;
  stripeId: string;
  idEntrenador: number;
  player: Player[];
  entrenador: entrenador;
  zone: string;
  lenguaje: string;
  status: string;
  idPlan: string;
  createdOn?: Date;
  updatedOn?: Date;
}
```

Example payload:

```json
{
{
  "id": 987654,
  "empresa": "EmpresaUA",
  "header": "Cabecera",
  "pin": "1234",
  "phoneNumber": "1234456768",
  "profile": 2,
  "profileStr": "User",
  "user": "usuario456",
  "nombre": "Ana",
  "lastName": "Gómez",
  "idEntrenador": 543456,
  "entrenador":{
    "nombre":"Manuel",
    "apellido":"Laoz"
  },
  "player": [
    {
      "id": "234324",
      "firstName": "Messi"
    },
    {
      "id": "2343244",
      "firstName": "Ronaldo"
    }
  ],
  "zone": "Ejemplo de Zona",
  "lenguaje": "EN",
  "status": "inactivo",
  "idPlan": "456"
}

}
```

:heavy_check_mark: 201 Ok Created
:x: 403 Unauthorized
:x: 422 User is mandatory
:x: 422 User pass is mandatory

# Evaluation Criteria

## Actions for criteria controller

| Verb   | URI                   | Action  | Route Name                          | Response Type                  |
| ------ | --------------------- | ------- | ----------------------------------- | ------------------------------ |
| GET    | `api/2/criteria`      | list    | `EvalCriterionController#list()`    | [`ListWCursor<T>`](#responses) |
| GET    | `api/2/criteria/{id}` | single  | `EvalCriterionController#single()`  | `<T>`                          |

Here, `<T>` equals to the `Type`, and in this particular case is `EvalCriterionDomain` (defined [below](#type-definition-criterion))

### Type definition criterion

```ts
interface Criterion {
  id: long;
  tag: string;
  type: "check" | "likert" | "scale";
  /**
   * Represents the max value for the given scale (from 0 to 'scale' value)
   */
  scale?: integer; // <- Optional, only if type is 'scale' 
  createdOn?: string; // <- must be a Date (yyyy-MM-dd'T'HH:mm:ssXXX)
  updatedOn?: string; // <- must be a Date (yyyy-MM-dd'T'HH:mm:ssXXX)
}
```

Example payload:

```json
{
  "tag": "velocidad",
  "type": "scale",
  "scale": 100
}
```

:heavy_check_mark: 201 Ok Created
:x: 403 Unauthorized
:x: 422 Tag is required by criterion
:x: 422 Scale is required by scale criterion
:x: 422 CriterionType is undefined

# OrgControllerV2

## Actions for Organization controller

| Verb   | URI                               | Action             | Route Name                             | Response Type                  |
| ------ | --------------------------------- | ------------------ | -------------------------------------- | ------------------------------ |
| GET    | `api/2/orgs`                      | list               | `OrgControllerV2#list()`               | [`ListWCursor<T>`](#responses) |
| GET    | `api/2/orgs/{id}`                 | single             | `OrgControllerV2#single()`             | `<T>`                          |

Here, `<T>` equals to the `Type`, and in this particular case is  `OrgDomain` (defined [below](#type-definition-orgcontrollerv2))

### Type definition OrgControllerV2

```ts
interface OrgDomain {
  id: number;
  namespace: string;
  nombre: string;
  rfc: string;
  telefono: string;
  clasificacion: string;
  contacto: string;
  calle: string;
  num_ext: int;
  num_int: int;
  pais: int;
  estado: int;
  localidad: int;
  cp: int;
  zona: string;
  latitud: double;
  longitud: double;
  idPlan: string;
  namePlan: string;
  teams: int;
  players: int;
  editPlan: boolean;
  escudo: string; 
  urlLogo: string;
  createdOn?: Date;
  updatedOn?: Date;
}

```

Example payload:

```json
{
{
  "id": 12345,
  "namespace": "Pedro-co",
  "nombre": "Pedro",
  "rfc": "EXA123456789",
  "telefono": "123456789",
  "clasificacion": "Categoría A",
  "contacto": "contacto@ejemplo.com",
  "calle": "Calle 13 La conquista",
  "num_ext": 100,
  "num_int": 5,
  "pais": 1,
  "estado": 2,
  "localidad": 3,
  "cp": 12345,
  "zona": "Zona Residencial",
  "latitud": 19.4326,
  "longitud": -99.1332,
  "idPlan": "34123",
  "namePlan": "Plan de Ejemplo",
  "teams": 10,
  "players": 30,
  "editPlan": true,
  "escudo": "escudoEjemplo.png",
  "urlLogo": "http://www.ejemplo.com/logo.png"
}

}
```

:heavy_check_mark: 201 Ok Created
:x: 403 Unauthorized
:x: 422 Organization Type is undefined
:x: 422 Name is required by new observable behavior
:x: 422 Description is required by new observable behavior

# School

## Actions for School controller

| Verb   | URI                  | Action  | Route Name                          | Response Type                  |
| ------ | -------------------- | ------- | ----------------------------------- | ------------------------------ |
| GET    | `api/2/schools`      | list    | `SchoolController#list()`           | [`ListWCursor<T>`](#responses) |
| GET    | `api/2/schools/{id}` | single  | `SchoolController#single()`         | `<T>`                          |

Here, `<T>` equals to the `Type`, and in this particular case is `EscuelaDomain` (defined [below](#type-definition-school))

### Type definition School

```ts
interface School{
  id: number;
  nombre: string;
  clave: string;
  idEscuela: string;
  telefono: string;
  contacto: string;
  calle: string;
  num_ext: int;
  num_int: int;
  pais: int;
  estado: int;
  localidad: int;
  colonia: string;
  cp: int;
  latitud: double;
  longitud: double;
  createdOn?: Date;
  updatedOn?: Date;
}
```

Example payload:

```json
{
  "id": 12345,
  "nombre" : "nombre5",
  "clave" : "clave",
  "rfc" : "rfc",
  "telefono" : "telefono",
  "contacto" : "contacto",
  "calle" : "calle",
  "num_ext" : 23,
  "num_int" : 11,
  "pais" : "pais",
  "estado" : 2,
  "localidad" : "localidad",
  "colonia" : "colonia",
  "cp" : 0,
  "latitud" : 234,
  "longitud" : 567
}
```

:heavy_check_mark: 201 Ok Created
:x: 403 Unauthorized
:x: 422 School Type is undefined
:x: 422 Clave is required by new School
:x: 422 Clave is required by new School
:x: 422 Name is required by new School
:x: 422 Contact is required by new School

# Campus

## Actions for Campus controller

| Verb   | URI                   | Action  | Route Name                          | Response Type                  |
| ------ | --------------------- | ------- | ----------------------------------- | ------------------------------ |
| GET    | `api/2/campuses`      | list    | `CampusController#list()`           | [`ListWCursor<T>`](#responses) |
| GET    | `api/2/campuses/{id}` | single  | `CampusController#single()`         | `<T>`                          |

Here, `<T>` equals to the `Type`, and in this particular case is `SedeDomain` (defined [below](#type-definition-campus))

### Type definition Campus

```ts
interface Campus{
  id: number;
  nombre: string;
  clave: string;
  idEscuela: number;
  telefono: string;
  contacto: string;
  calle?: string;
  num_ext: int;
  num_int: int;
  pais: string;
  estado: int;
  localidad?: string;
  colonia?: string;
  cp: int;
  latitud: double;
  longitud: double;
  createdOn?: Date;
  updateOn?: Date;
}
```

Example payload:

```json
{
	"nombre" : "nombre5",
	"clave" : "clave",
	"idEscuela" : 11118,
	"telefono" : "123445678",
	"contacto" : "contacto",
	"calle" : "calle",
	"num_ext" : 23,
	"num_int" : 11,
	"pais" : "pais",
	"estado" : 2,
	"localidad" : "localidad",
	"colonia" : "colonia",
	"cp" : 0,
	"latitud" : 234,
	"longitud" : 567
}
```

:heavy_check_mark: 201 Ok Created
:x: 403 Unauthorized
:x: 422 Campus Type is undefined
:x: 422 Clave is required by new Campus
:x: 422 Name is required by new Campus
:x: 422 Contact is required by new Campus
:x: 422 School is required by new Campus

# Coach

## Actions for Coach controller

| Verb   | URI                 | Action  | Route Name                          | Response Type                  |
| ------ | ------------------- | ------- | ----------------------------------- | ------------------------------ |
| GET    | `api/2/coachs`      | list    | `CoachController#list()`            | [`ListWCursor<T>`](#responses) |
| GET    | `api/2/coachs/{id}` | single  | `CoachController#single()`          | `<T>`                          |

Here, `<T>` equals to the `Type`, and in this particular case is `EntrenadorDomain` (defined [below](#type-definition-coach))

### Type definition Coach

```ts
interface Coach {
  id: long;
  nombre: string;
  apellidos: string;
  domicilio: string;
  telefono: string;
  curp: string;
  fechaNac: string; 
  correo: string;
  perfilEntrenador: string;
  idGrupo: string[]; 
  teams: GrupoDomain[]; 
  idUser: string;
  id_img: string;
  nationality: string;
  namespace: string;
  status: int;
  createdOn?: Date;
  updateOn ?: Date;
}
```

Example payload:

```json
{
  "nombre": "Juan",
  "apellidos": "Pérez",
  "domicilio": "Calle Principal 123",
  "telefono": "123456789",
  "curp": "JUAP920101HDFRNN09",
  "fechaNac": "1995-07-15", 
  "correo": "juan.perez@ejemplo.com",
  "perfilEntrenador": "Entrenador de Fútbol",
  "idGrupo": ["23401", "23402"],
  "teams": [
    {
      "id": "34201",
      "nombre": "Equipo A"
    },
    {
      "id": "23402",
      "nombre": "Equipo B"
    }
  ],
  "idUser": "2343223",
  "id_img": "23432",
  "nationality": "Mexicana",
  "namespace": "ejemploNamespace",
  "status": 1
}

```

:heavy_check_mark: 201 Ok Created
:x: 403 Unauthorized
:x: 422 Coach Type is undefined
:x: 422 Name is required by new Coach
:x: 422 Date of birth is required by new Coach
:x: 422 Curp is required by new Coach

# Team

## Actions for Team controller

| Verb   | URI                                 | Action           | Route Name                          | Response Type                         |
| ------ | ----------------------------------- | ---------------- | ----------------------------------- | ------------------------------------- |
| GET    | `api/2/teams`                       | list             | `TeamController#list()`             | [`ListWCursor<T>`](#responses)        |
| GET    | `api/2/teams/{id}`                  | single           | `TeamController#single()`           | `<T>`                                 |
| GET    | `api/2/teams/get_team_ob/{nm}`      | get_team_ob      | `TeamController#get_team_ob()`      | `<T>`                                 |

Here, `<T>` equals to the `Type`, and in this particular case is `GrupoDomain` (defined [below](#type-definition-team))

### Type definition Team

```ts
interface Team {
  id: long;
  clave: string;
  nombre: string;
  descripcion: string;
  observaciones: string;
  id_img?: string;
  idSede: long;
  idPlan: long;
  idTemporada: long;
  plan: string;
  idCateg: long;
  idEntrenador: long;
  estatusPlanCargado: int;
  iniDate: string | Date;
  numeroJugadores: int;
  categoria: Category;
  sede: Venues;
  entrenador: entrenador;
  temporada: Season;
  enrollmentConfig: EnrollmentConfig;
  lastPlanContentLoadedId: int;
  createdOn?: Date;
  updateOn?: Date;
}
```

Example payload:

```json
{
  "id": 2345,
  "clave": "clave123",
  "nombre": "Nombre05",
  "descripcion": "Descripción del equipo",
  "observaciones": "Observaciones adicionales sobre el equipo",
  "id_img": "42343",
  "idSede": 1,
  "idPlan": 101,
  "idTemporada": 2024,
  "plan": "Plan de Ejemplo",
  "idCateg": 5,
  "idEntrenador": 123,
  "estatusPlanCargado": 2,
  "iniDate": "2024-01-01", 
  "numeroJugadores": 25,
  "categoria": {
    "id": 1,
    "nombre": "Categoría A"
  },
  "sede": {
    "id": 1,
    "nombre": "Sede Central"
  },
  "entrenador": {
    "id": "324001",
    "nombre": "Juan",
    "apellidos": "Pérez",
    "teams": [
      {
        "id": "7891",
        "nombre": "Equipo A"
      },
      {
        "id": "78902",
        "nombre": "Equipo B"
      }
    ],
    "idUser": "23423",
    "id_img": "243324",
    "nationality": "Mexicana",
    "namespace": "ejemploNamespace",
    "status": 1
  },
  "temporada": {
    "id": 2024,
    "nombre": "Temporada 2024"
  },
  "enrollmentConfig": {
    "id": 1,
    "configuracion": "Configuración Ejemplo"
  },
  "lastPlanContentLoadedId": 202
}

```

:heavy_check_mark: 201 Ok Created
:x: 403 Unauthorized
:x: 422 Team Type is undefined
:x: 422 Name is required by new Team
:x: 422 Clave of birth is required by new Coach
:x: 422 Descripcion is required by new Coach
:x: 422 Category is required by new Coach
:x: 422 Coach of birth is required by new Coach
:x: 422 Campus is required by new Coach

# Player

## Actions for Player controller

|Verb  |URI                                                       |Action                 |Route Name                                  |Response Type                    |
|------|--------------------------------------------------------- |-----------------------|--------------------------------------------|---------------------------------|
|GET   |`api/2/players`                                           |list                   |`PlayerController#list()`                   |[`ListWCursor<T>`](#responses)   |
|GET   |`api/2/players/{id}`                                      |single                 |`PlayerController#single()`                 |`<T>`                            |
|GET   |`api/2/players/{id}/statistics`                           |statistics             |`PlayerController#statistics()`             |[`ListWCursor<PlayerStatistics>`]|
|GET   |`api/2/players/match-statistics`                          |matchStatistics        |`PlayerController#matchStatistics()`        |`PlayerMatchStatistics`          |
|GET   |`api/2/players/comments-by-player/{id}`                   |commentsByPlayer       |`PlayerController#commentsByPlayer()`       |[`ListWCursor<CommentByPlayer>`] |
|GET   |`api/2/players/current-season-players/{seasonId}/{teamId}`|getCurrentSeasonPlayers|`PlayerController#getCurrentSeasonPlayers()`|[`ListWCursor<T>`]               |

Here, `<T>` equals to the `Type`, and in this particular case is `Player` (defined [below](#type-definition-player))

### Type definition Player

```ts
interface Player {	
  id: long;
  nombre: string;
  firstName: string;
  lastName: string;
  matricula: string;
  matricula_fed: string;
  domicilio: string;
  telefono: string;
  curp: string;
  fechaNac: string;
  provenance_team: string;
  nombrePadre: string;
  telPadre: string;
  nombreMadre: string;
  telMadre: string;
  perfilJugador: string;
  observaciones: string;
  idGrupo: string;
  idUser: string;
  grupo: string;
  id_img: string;
  correo: string;
  IdAntropometrico: string;
  posicionJugador: string;
  activo: int; 
  createdOn?: Date;
  updateOn?: Date;
}
```

Example payload:

```json
{
  "id": "12345",
  "nombre": "Juan",
  "firstName": "Juan",
  "lastName": "Pérez",
  "matricula": "A123456",
  "matricula_fed": "FED123456",
  "domicilio": "Calle Falsa 123",
  "telefono": "123456789",
  "curp": "JUPJ800101HDFRRN01",
  "fechaNac": "2000-01-01",
  "provenance_team": "Equipo A",
  "nombrePadre": "José Pérez",
  "telPadre": "123456789",
  "nombreMadre": "María Pérez",
  "telMadre": "555-8765",
  "perfilJugador": "Defensor",
  "observaciones": "Jugador destacado",
  "idGrupo": "7891",
  "idUser": "7897893",
  "grupo": "Grupo A",
  "id_img": "5678",
  "correo": "juan.perez@gmail.com",
  "IdAntropometrico": "antropo123",
  "posicionJugador": "Defensa",
  "activo": 1
}
```

:heavy_check_mark: 201 Ok Created
:x: 403 Unauthorized
:x: 422 Player Type is undefined
:x: 422 Name is required by new player
:x: 422 Date of bird of birth is required by new player
:x: 422 Curp is required by new player
:x: 422 Federative enrollment is required by new player

# Training Plan V2

## Actions for Training Plan controller

| Verb   | URI                           | Action  | Route Name                           | Response Type                  |
| ------ | ----------------------------- | ------- | ------------------------------------ | ------------------------------ |
| GET    | `api/2/training-plans`        | list    | `TrainingPlanControllerV2#list()`    | [`ListWCursor<T>`](#responses) |
| GET    | `api/2/training-plans/{id}`   | single  | `TrainingPlanControllerV2#single()`  | `<T>`                          |

### Type definition TrainingPlan

```ts
interface TrainingPlan {
 id: long;
  nombre: string;
  identif: string;
  descripcion: string;
  observ: string;
  tiempo_recom_sesion: numbinter; 
  idGameModel: string;
  gameModel: string;
  createdBy: string;
  duracion: int;
  status: string;
  idCateg: string;
  categoria: string;
  categoryIds: string[];
  jsonPlan: string; /
  htmlPLan: string; 
  a: string;
  b: string;
  estJson: string;
  isLoaded: boolean;
  sessionsWeek: int;
  locked: boolean;
  planClassificationId: string;
  planClassificationIds: string[];
  type: string; 
  createdOn?: Date;
  updateOn?: Date;
}
```

Example payload:

```json
{
 {
  "id": "3454323",
  "nombre": "Plan de Entrenamiento A",
  "identif": "PLAN-A-001",
  "descripcion": "Este es un plan de entrenamiento avanzado.",
  "observ": "Requiere supervisión constante.",
  "tiempo_recom_sesion": 60,
  "idGameModel": "gameModel123",
  "gameModel": "Fútbol",
  "createdBy": "admin",
  "duracion": 120,
  "status": "activo",
  "idCateg": "23401",
  "categoria": "Categoría A",
  "categoryIds": ["001", "002", "003"],
  "jsonPlan": "json",
  "htmlPLan": "html",
  "a": "a",
  "b": "b",
  "estJson": "estJson",
  "isLoaded": true,
  "sessionsWeek": 3,
  "locked": false,
  "planClassificationId": "001",
  "planClassificationIds": ["001", "002"],
  "type": "custom"
}
}
```

:heavy_check_mark: 201 Ok Created
:x: 403 Unauthorized
:x: 422 TrainingPlan Type is undefined
:x: 422 Name is required by new Training Plan
:x: 422 Categories is required for new Training Plan
:x: 422 Descripcion is required by new Training Plan
:x: 422 Recommended Session Time is required by new Training Plan
:x: 422 Game Model is required by new Training Plan

# Macrociclo

## Actions for Macrociclo controller

| Verb   | URI                      | Action  | Route Name                          | Response Type                  |
| ------ | ------------------------ | ------- | ----------------------------------- | ------------------------------ |
| GET    | `api/2/macrociclos`      | list    | `MacrocicloController#list()`       | [`ListWCursor<T>`](#responses) |
| GET    | `api/2/macrociclos/{id}` | single  | `MacrocicloController#single()`     | `<T>`                          |

Here, `<T>` equals to the `Type`, and in this particular case is `MacrocicloDomain` (defined [below](#type-definition-macrociclo))

### Type definition Macrociclo

```ts
interface Macrociclo {	
  idMacro: string;
  duracion: int;
  macroPosition: int;
  nombre: string;
  descripcion: string;
  idAntiguo: string;
  idNuevo: string;
  idTrainingPlan: string;
  lista_meso: Mesociclo[];
  createdOn?: Date;
  updateOn?: Date;
}
```

Example payload:

```json
{	
  "idMacro":"76878",
  "duracion": 90,
  "macroPosition": 1,
  "nombre": "Macro Plan de Entrenamiento",
  "descripcion": "Este es un macro plan de entrenamiento enfocado en la resistencia.",
  "idAntiguo": "antiguo123",
  "idNuevo": "89456",
  "idTrainingPlan": "09789"
  
}
```

:heavy_check_mark: 201 Ok Created
:x: 403 Unauthorized
:x: 422 Macrociclo Type is undefined
:x: 422 Name is required by new Macrociclo
:x: 422 Descripcion is required by new Macrociclo

# Mesociclo

## Actions for Mesociclo controller

| Verb   | URI                     | Action  | Route Name                          | Response Type                  |
| ------ | ----------------------- | ------- | ----------------------------------- | ------------------------------ |
| GET    | `api/2/mesociclos`      | list    | `MesocicloController#list()`        | [`ListWCursor<T>`](#responses) |
| GET    | `api/2/mesociclos/{id}` | single  | `MesocicloController#single()`      | `<T>`                          |

Here, `<T>` equals to the `Type`, and in this particular case is `MesocicloDomain` (defined [below](#type-definition-mesociclo))

### Type definition Mesociclo

```ts
interface Mesociclo {	
  idMeso: string;
  duracion: int;
  macroPosition: int;
  mesoPosition: int;
  nombre: string;
  descripcion: string;
  idMacro: string;
  idAntiguo: string;
  idNuevo: string;
  lista_micro: Microciclo[];
  createdOn?: Date;
  updateOn?: Date;
}
```

Example payload:

```json
{
  "idMeso": "20",
  "duracion": 60,
  "macroPosition": 1,
  "mesoPosition": 2,
  "nombre": "Meso Ciclo de Fuerza",
  "descripcion": "Este es un meso ciclo enfocado en el desarrollo de fuerza.",
  "idMacro": "101",
  "idAntiguo": "19",
  "idNuevo": "30"
}

```

:heavy_check_mark: 201 Ok Created
:x: 403 Unauthorized
:x: 422 Mesociclo Type is undefined
:x: 422 Name is required by new Mesociclo
:x: 422 Macrociclo is required by new Mesociclo
:x: 422 Descripcion is required by new Mesociclo

# Microciclo

## Actions for Microciclo controller

| Verb   | URI                      | Action  | Route Name                          | Response Type                  |
| ------ | ------------------------ | ------- | ----------------------------------- | ------------------------------ |
| GET    | `api/2/microciclos`      | list    | `MicrocicloController#list()`       | [`ListWCursor<T>`](#responses) |
| GET    | `api/2/microciclos/{id}` | single  | `MicrocicloController#single()`     | `<T>`                          |

Here, `<T>` equals to the `Type`, and in this particular case is `MicrocicloDomain` (defined [below](#type-definition-microciclo))

### Type definition Microciclo

```ts
interface Microciclo {	
  idMicro: string;
  duracion: int;
  macroPosition: int;
  mesoPosition: int;
  microPosition: int;
  nombre: string;
  descripcion: string;
  idMacro: string;
  idMeso: string;
  idAntiguo: string;
  idNuevo: string;
  lista_sesion: Sesion[];
  createdOn?: Date;
  updateOn?: Date;
}
```

Example payload:

```json
{
  "idMicro": "303",
  "duracion": 30,
  "macroPosition": 1,
  "mesoPosition": 2,
  "microPosition": 3,
  "nombre": "Micro Ciclo de Resistencia",
  "descripcion": "Este es un micro ciclo enfocado en mejorar la resistencia cardiovascular.",
  "idMacro": "101",
  "idMeso": "202",
  "idAntiguo": "4565",
  "idNuevo": "645678"
}

```

:heavy_check_mark: 201 Ok Created
:x: 403 Unauthorized
:x: 422 Microciclo Type is undefined
:x: 422 Name is required by new Microciclo
:x: 422 Descripcion is required by new Microciclo
:x: 422 IdMeso is required by new Microciclo
:x: 422 IdMacro is required by new Microciclo

# GameModel V2

## Actions for GameModel controller

| Verb   | URI                      | Action  | Route Name                          | Response Type                  |
| ------ | ------------------------ | ------- | ----------------------------------- | ------------------------------ |
| GET    | `api/2/game-models`      | list    | `GameModelControllerV2#list()`      | [`ListWCursor<T>`](#responses) |
| GET    | `api/2/game-models/{id}` | single  | `GameModelControllerV2#single()`    | `<T>`                          |

Here, `<T>` equals to the `Type`, and in this particular case is `GameModelDomain` (defined [below](#type-definition-gamemodel))


### Type definition GameModel

```ts
interface GameModel {	
  id: long;
	nombre: string;
	identif: string;
	descripcion: string;
	observaciones: string;
  createdOn?: Date;
  updateOn?: Date;
}
```

Example payload:

```json
{
    "id":"7878",
  	"nombre": "pedro",
	  "identif": "pedro234",
    "descripcion": "rapido",
    "observaciones": "habilidoso",
}
```

:heavy_check_mark: 201 Ok Created
:x: 403 Unauthorized
:x: 422 GameModel Type is undefined
:x: 422 Name is required by new GameModel
:x: 422 Identify of birth is required by new GameModel
:x: 422 Descripcion is required by new GameModel

# Category V2

## Actions for Category controller

| Verb   | URI                         | Action              | Route Name                                   | Response Type                  |
| ------ | --------------------------- | ------------------- | -------------------------------------------- | ------------------------------ |
| GET    | `api/2/categories`          | list                | `CategoryControllerV2#list()`                | [`ListWCursor<T>`](#responses) |
| GET    | `api/2/categories/{id}`     | single              | `CategoryControllerV2#single()`              | `<T>`                          |

Here, `<T>` equals to the `Type`, and in this particular case is `CategoriesDomain` (defined [below](#type-definition-category))

### Type definition Category

```ts
interface Category {	
  id: long;
  nombre: string;
  nombre_univ: string;
  identif: string;
  edad_min: int;
  edad_max: int;
  criterion?: Criterion;
  createdOn?: Date;
  updateOn?: Date;
}
```

Example payload:

```json
{
    "id":"7878",
    "nombre": "rapides",
    "nombre_univ": "rapides 2",
    "identif": "rapi2341",
    "edad_min": "15",
    "edad_max": "24",
    "criterion": {
      "tag": "velocidad",
      "type": "scale",
      "scale": 100
    },
}
```

:heavy_check_mark: 201 Ok Created
:x: 403 Unauthorized
:x: 422 Category Type is undefined
:x: 422 Name is required by new Category
:x: 422 Identification is required by new Category
:x: 422 Universal Name is required by new Category
:x: 422 minimum age is required by new Category
:x: 422 maximum age is required by new Category
:x: 422 maximum age needs to be older than the minimum age

# Observables_Patterns V2

## Actions for Observables_Patterns controller

|Verb  |URI                                                  |Action                          |Route Name                                                 |Response Type                                   |
|------|-----------------------------------------------------|--------------------------------|-----------------------------------------------------------|------------------------------------------------|
|GET   |`api/2/ob-patterns`                                  |list                            |`ObPatternsControllerV2#list()`                            |[`ListWCursor<T>`](#responses)                  |
|GET   |`api/2/ob-patterns/{id}`                             |single                          |`ObPatternsControllerV2#single()`                          |`<T>`                                           |
|GET   |`api/2/ob-patterns/query-mobile/{scheduledSessionId}`|getBehaviorsByScheduledSessionId|`ObPatternsControllerV2#getBehaviorsByScheduledSessionId()`|[`ListWCursor<ObservableBehaviorsByCompetency>`]|

Here, `<T>` equals to the `Type`, and in this particular case is `ObPatternsDomain` (defined [below](#type-definition-observables_patterns))

### Type definition Observables_Patterns

```ts
interface Observables_Patterns {	
  id: string,
	idCompetency: string,
	nombre: string,
	identif: string,
	descripcion: string,
	observaciones: string,
	[] categorias: string,
	niveles: string,
	id_img: string,
	id_video: string,
	activo: int,
	createdOn: Date,
	updatedOn: Date,
}
```

Example payload:

```json
  {
    "id":"6767",
    "idCompetency": "2345",
	  "nombre": "competenica correr",
	  "identif": "234",
	  "descripcion": "tecnica de correr",
	  "observaciones": "actitud",
	  "categorias": ["categoria1"],
	  "niveles": "niveles ejemplo",
	  "id_img": "34435",
	  "id_video": "3444",
	  "activo": 1,
  }
```

:heavy_check_mark: 201 Ok Created
:x: 403 Unauthorized
:x: 422 Observables_Patterns Type is undefined
:x: 422 Name is required by new observable behavior
:x: 422 Description is required by new observable behavior
:x: 422 category relations are required by new observable behavior

# Task

## Actions for Task controller

| Verb   | URI                             | Action             | Route Name                            | Response Type                  |
| ------ | ------------------------------- | ------------------ | ------------------------------------- | ------------------------------ |
| GET    | `api/2/tasks`                   | list               | `TaskController#list()`               | [`ListWCursor<T>`](#responses) |
| GET    | `api/2/tasks/{id}`              | single             | `TaskController#single()`             | `<T>`                          |

Here, `<T>` equals to the `Type`, and in this particular case is `TareasDomain` (defined [below](#type-definition-task))

### Type definition Task

```ts
interface Task {	
  id: long;
	nombre: string;
	identif: string;
	descripcion: string;
	observ: string;
	superficie: string;
	num_jugadores: string;
  zone: string;
  type_task: string;
	duracion: string;
	compet_principal: int;
	modelo_juego: int[];
	compet_secunda: int[];
	objetivos: string [];
	variantes: string [];
	conductas: string [];
  materiales: string [];
	isFinalizado: boolean;
	totalTareas: int;
	id_video: string;
	id_img: string;
  sesionObjId: string;
  nameModelo: string [];
  nameCompetSec: string [];
	nameCompetPrin: string;
	createdOn: Date;
	updatedOn: Date;
}
```

Example payload:

```json
  {
    "id":"898989",
    "nombre": "nombreTarea",
	  "identif": "TareaIdenti",
	  "descripcion": "Descripcion de tarea",
	  "observ": "observaciones",
	  "superficie": "1243 metros",
	  "num_jugadores": "20",
    "zone": "Ejemplo Zona",
    "type_task": "Prioritaria",
	  "duracion": "90 min",
	  "compet_principal": 443234,
	  "modelo_juego": [3221, 43412],
	  "compet_secunda": [33124],
	  "objetivos": ["Mejorar"],
	  "variantes": ["tiempo"],
	  "conductas": ["proactivo"],
    "materiales": ["conos,pesas,escaleras"],
	  "isFinalizado": false,
	  "totalTareas": 4,
	  "id_video": "432122",
	  "id_img": "1243f3",
    "sesionObjId": "1234",
    "nameModelo": ["ejemModel"],
    "nameCompetSec": ["ejempcomp"],
	  "nameCompetPrin": "CompetenciasiR"
  }
```

:heavy_check_mark: 201 Ok Created
:x: 403 Unauthorized
:x: 422 Task Type is undefined
:x: 422 Name is required by new Task
:x: 422 identification is required by new Task
:x: 422 Descripcion is required by new Task
:x: 422 Time is required by new Task
:x: 422 Number of players is required by new Task
:x: 422 Game model is required by new Task
:x: 422 main competition is required by new Task

# Sesion V2

## Actions for Sesion controller

| Verb   | URI                              | Action       | Route Name                          | Response Type                   |
| ------ | -------------------------------- | -------------| ----------------------------------- | ------------------------------  |
| GET    | `api/2/sesions`                 | list          | `SesionControllerV2#list()`          | [`ListWCursor<T>`](#responses) |
| GET    | `api/2/sesions/{id}`            | single        | `SesionControllerV2#single()`        | `<T>`                          |
| GET    | `api/2/sesions/objs/{id}/`      | objs          | `SesionControllerV2#objs()`          | `SesionObjectDomain`           |


Here, `<T>` equals to the `Type`, and in this particular case is `SesionDomain` (defined [below](#type-definition-sesion))

### Type definition Sesion

```ts
interface Sesion {	
  id: string,
  idGrupo: string,
  duracion: string,
  tipoSesion: string, 'normal' | 'agenda-extra' | 'plan-extra';
  claseSesion: string,
  mesoPosition: string,
  microPosition: string,
  macroPosition: string,
  sesionPosition: string,
  idPlanEntrenamiento: string,
  sede: string,
  grupo: string,
  fecha: string,
  nombre: string,
  idMacro: string,
  idMeso: string,
  idMicro: string,
  idSesion: string,
  horaIni: string,
  horaFin: string,
  categoria: string,
  temporada: string,
  observacion: string,
  descripcion: string,
  clasificacion: string,
  identificacion: string,
  idAntiguo: string,
  idNuevo: string,
  order: int,
  ejecucion?: boolean,
  createdOn?: boolenan,
  updatedOn?: boolenan,
}
```

Example payload:

```json
  {
  "id": "234234234234",
  "idGrupo": "23423423523" ,
  "duracion": "15" ,
  "tipoSesion": "normal", 
  "claseSesion": "ejemplo de clase",
  "mesoPosition": "1",
  "microPosition": "2",
  "macroPosition": "3",
  "sesionPosition": "3",
  "idPlanEntrenamiento": "124123123124",
  "sede": "campus V",
  "grupo": "delfines",
  "fecha": "03/06/23",
  "nombre": "sesion de velocidad",
  "idMacro": "449383",
  "idMeso": "948939",
  "idMicro": "94839",
  "idSesion": "23423423435",
  "horaIni": "10:30",
  "horaFin": "12:30",
  "categoria": "sub 15",
  "temporada": "enero-junio",
  "observacion": "dia caluroso",
  "descripcion": "sesion de velocidad",
  "clasificacion": "velocidad",
  "identificacion": "2234883423",
  "idAntiguo": "8382938493948",
  "idNuevo": "832388238989238",
  "order": 12,
  "ejecucion": true,
    },
}
```

:heavy_check_mark: 201 Ok Created
:x: 403 Unauthorized
:x: 422 Sesion Type is undefined
:x: 422 Descripcion is required by new Sesion
:x: 422 Nombre is required by new Sesion
:x: 422 identif is required by new Sesion
:x: 422 Clasification is required by new Sesion

# Competencies

## Actions for Competencies controller

| Verb   | URI                           | Action             | Route Name                                   | Response Type                  |
| ------ | ----------------------------- | ------------------ | -------------------------------------------- | ------------------------------ |
| GET    | `api/2/competencies`          | list               | `CompetitionController#list()`               | [`ListWCursor<T>`](#responses) |
| GET    | `api/2/competencies/{id}`     | single             | `CompetitionController#single()`             | `<T>`                          |

Here, `<T>` equals to the `Type`, and in this particular case is `CompetenciesDomain` (defined [below](#type-definition-competition))

### Type definition Competition

```ts
interface Competition {	
  id?: string,
	nombre: string,
	identif: string,
	descripcion: string,
  principleId: string,
	clasificacion1: string,
	clasificacion2: string,
	clasificacion3: string,
  comportamientos: string [],
	id_video?: string,
	id_img?: string,
	createdOn?: Date,
	updatedOn?: Date,
}
```

Example payload:

```json
  {
    "id": "7646373823734",
    "nombre": "competencia resistencia",
    "identif": "comp resis",
    "descripcion": "el deportista aumentara su resistencia",
    "principleId": "231",
    "clasificacion1": "Generica",
    "clasificacion2": "Humana-social",
    "clasificacion3": "Intermedia",
    "comportamientos": ["EjemComportamientos"],
    "id_video": "34235335",
    "id_img": "344992849",
  }
```

:heavy_check_mark: 201 Ok Created
:x: 403 Unauthorized
:x: 422 Competition Type is undefined
:x: 422 Name is required by new Competition
:x: 422 identification is required by new Competition
:x: 422 Description is required by new Competition
:x: 422 principleId is required by new Competition
:x: 422 type enrollment is required by new Competition

# Activities

## Actions for Activities controller

| Verb   | URI                     | Action  | Route Name                     | Response Type                  |
| ------ | ----------------------- | ------- | ------------------------------ | ------------------------------ |
| GET    | `api/2/activities`      | list    | `ActivityController#list()`    | [`ListWCursor<T>`](#responses) |
| GET    | `api/2/activities/{id}` | single  | `ActivityController#single()`  | `<T>`                          |

Here, `<T>` equals to the `Type`, and in this particular case is `Activity` (defined [below](#type-definition-competition))

### Type definition Activity

```ts
interface Activity {	
  id: long;
  description: string;
  obs: string;
  createdOn?: Date,
	updatedOn?: Date
}
```

Example payload:

```json
  {
  
  "description": "Este es un objeto simple.",
  "obs": "Observaciones adicionales sobre el objeto."

}
```

:heavy_check_mark: 201 Ok Created
:x: 403 Unauthorized
:x: 422 Activity type description is mandatory


# Activities-Log

## Actions for Activities-log controller

| Verb   | URI                         | Action  | Route Name                        | Response Type                  |
| ------ | --------------------------- | ------- | --------------------------------- | ------------------------------ |
| GET    | `api/2/activities-log`      | list    | `ActivityLogController#list()`    | [`ListWCursor<T>`](#responses) |
| GET    | `api/2/activities-log/{id}` | single  | `ActivityLogController#single()`  | `<T>`                          |

Here, `<T>` equals to the `Type`, and in this particular case is `ActivityLog` (defined [below](#type-definition-activitylog))

### Type definition ActivityLog

```ts
interface ActivityLog {	
  id: long;
  seasonId: string;
  playerId: string;
  teamId: string;
  userId: string;
  reasonId: string;
  description: string;
  location: string;
  latitude: double;
  longitude: double;
  date: Date;
  observations: string;
  player?: IPlayer;
  createdOn?: Date,
	updatedOn?: Date,
}
```

Example payload:

```json
  {
  "id": "12345",
  "seasonId": "2023-2024",
  "playerId": "67890",
  "teamId": "54321",
  "userId": "11111",
  "reasonId": "22222",
  "description": "Descripción del evento",
  "location": "Ciudad, País",
  "latitude": 40.7128,
  "longitude": -74.0060,
  "date": "2024-08-19T12:34:56Z",
  "observations": "Observaciones adicionales",
  "player": {
    "firstName": "Lamine",
    "lastName": "Yamal",
    "posicionJugador": "Delantero",
    
  }

}
```

:heavy_check_mark: 201 Ok Created
:x: 403 Unauthorized
:x: 422 Activity-log type description is mandatory

# Antropometricos

## Actions for Antropometricos controller

| Verb   | URI                          | Action  | Route Name                            | Response Type                  |
| ------ | ---------------------------- | ------- | ------------------------------------- | ------------------------------ |
| GET    | `api/2/antropometricos`      | list    | `AntropometricosController#list()`    | [`ListWCursor<T>`](#responses) |
| GET    | `api/2/antropometricos/{id}` | single  | `AntropometricosController#single()`  | `<T>`                          |

Here, `<T>` equals to the `Type`, and in this particular case is `AntropometricosDomain` (defined [below](#type-definition-antropometricos))

### Type definition Antropometricos

```ts
interface Antropometricos {	
  id: long;
  idJugador: string;
  altura: float;
  peso: float;
  imc: float;
  t: float;
  se: float;
  si: float;
  a: float;
  graso: float;
  createdOn?: Date,
	updatedOn?: Date,
}
```

Example payload:

```json
  {
  
  "id": "12345",
  "idJugador": "67890",
  "altura": 1.75,
  "peso": 70.5,
  "imc": 23.0,
  "t": 0.5,
  "se": 0.3,
  "si": 0.4,
  "a": 1.2,
  "graso": 15.0

}
```

:heavy_check_mark: 201 Ok Created
:x: 403 Unauthorized
:x: 422 idJugador is required by new AntropometricosDomain
:x: 422 altura is required by new AntropometricosDomain
:x: 422 peso is required by new AntropometricosDomain
:x: 422 imc is required by new AntropometricosDomain

# BuzonReporte

## Actions for BuzonReporte controller

| Verb   | URI                      | Action      | Route Name                             | Response Type                   |
| ------ | ------------------------ | ----------- | -------------------------------------- | ------------------------------- |
| GET    | `api/2/mailboxes`        | list        | `BuzonReporteController#list()`        | [`List<OrgDomain>`](#responses) |
| GET    | `api/2/mailboxes/{id}`   | single      | `BuzonReporteController#single()`      | `<T>`                           |


Here, `<T>` equals to the `Type`, and in this particular case is `ReportDomain` (defined [below](#type-definition-buzonreporte))

### Type definition BuzonReporte

```ts
interface Report {	
  id: long;
  user: string;
  attentionDate: Date;
  closeDate: Date;
  status: int;
  type: int;
  message: string;
  departmentId: string;
  reason: string;
  response: string;
  anonymous: boolean;
  folio: string,
  createdOn?: Date,
	updatedOn?: Date,
}
```

Example payload:

```json
  {
   "id": "1234",
  "user": "Kyam",
  "attentionDate": "2024-08-01",
  "closeDate": "2024-08-05",
  "status": 1,
  "type": 2,
  "message": "Ejemplo de Mensaje.",
  "departmentId": "12001",
  "reason": "Alguna razon",
  "response": "EjemResponse.",
  "anonymous": false,
  "folio": "123456789"
}
```

:heavy_check_mark: 201 Ok Created
:x: 403 Unauthorized

# CaseFiles Sesion Document

## Actions for CaseFilesSesionDocument controller

| Verb   | URI                                   | Action           | Route Name                                            | Response Type                  |
| ------ | ------------------------------------- | ---------------- | ----------------------------------------------------- | ------------------------------ |
| GET    | `api/2/documents-sesion`              | list             | `CasefileSesionDocumentController#list()`             | [`ListWCursor<T>`](#responses) |
| GET    | `api/2/documents-sesion/{id}`         | single           | `CasefileSesionDocumentController#single()`           | `<T>`                          |
| GET    | `api/2/documents-sesion/without`      | listWithout      | `CasefileSesionDocumentController#listWithout()`      | [`ListWCursor<T>`]             |


Here, `<T>` equals to the `Type`, and in this particular case is `CasefileSesionDocument` (defined [below](#type-definition-casefilessesiondocument))

### Type definition CaseFilesSesionDocument

```ts
interface CasefileSesionDocument {	
  id: long;
  sesionId: string;
  type: string;
  name: string;
  label: string;
  storaged: boolean;
  fileType: string;
  status: int;
  newVersion: boolean;
  base64: string;
  perfil: int;
  typeAttachment: string;
  url: string;
  createdOn?: Date,
	updatedOn?: Date,
}
```

Example payload:

```json
  {
   "id": "123",
  "sesionId": "456",
  "type": "Emjemplo tipo",
  "name": "Emeplo 1",
  "label": "Documento Label",
  "storaged": true,
  "fileType": ".pdf",
  "status": 1,
  "newVersion": false,
  "base64": "base64enasdjkasda0s9duia9wjdsa",
  "perfil": 2,
  "typeAttachment": "Attachment Type",
  "url": "https://ejemplo.com/documento",
}
```

:heavy_check_mark: 201 Ok Created
:x: 403 Unauthorized
:x: 422 Player field in Attachment is mandatory
:x: 422 Name field in Attachment is mandatory

# CaseFiles Document

## Actions for CasefileDocument controller

| Verb   | URI                                   | Action           | Route Name                                      | Response Type                  |
| ------ | ------------------------------------- | ---------------- | ----------------------------------------------- | ------------------------------ |
| GET    | `api/2/documents-player`              | list             | `CasefileDocumentController#list()`             | [`ListWCursor<T>`](#responses) |
| GET    | `api/2/documents-player/{id}`         | single           | `CasefileDocumentController#single()`           | `<T>`                          |
| GET    | `api/2/documents-sesion/without`      | listWithout      | `CasefileDocumentController#listWithout()`      | [`ListWCursor<T>`]             |

Here, `<T>` equals to the `Type`, and in this particular case is `CasefileDocument` (defined [below](#type-definition-casefiledocument))

### Type definition CasefileDocument

```ts
interface CasefileDocument {	
  id: long;
  playerId: string;
  player?: IPlayer;  
  type: string;
  nameId: string;
  label: string;
  storaged: boolean;
  fileType: string;
  status: int;
  newVersion: boolean;
  createdOn?: Date,
	updatedOn?: Date,
}
```

Example payload:

```json
  {
  
  "id": "123",
  "playerId": "456",
  "player": {
    "firstName": "Leo",
    "lastName": "Perez",
    "posicionJugador": "Delantero"
  },
  "type": "ejemploTipo",
  "nameId": "1001",
  "label": "EjemploLabel",
  "storaged": true,
  "fileType": ".pdf",
  "status": 2,
  "newVersion": false
}

```

:heavy_check_mark: 201 Ok Created
:x: 403 Unauthorized
:x: 422 Name field in Casefile Document is mandatory
:x: 422 File Type in Casefile Document is mandatory
:x: 422 Player field in Casefile Document is mandatory

# Classification

## Actions for Classification controller

| Verb   | URI                          | Action  | Route Name                           | Response Type                  |
| ------ | ---------------------------- | ------- | ------------------------------------ | ------------------------------ |
| GET    | `api/2/classifications`      | list    | `ClassificationController#list()`    | [`ListWCursor<T>`](#responses) |
| GET    | `api/2/classifications/{id}` | single  | `ClassificationController#single()`  | `<T>`                          |


Here, `<T>` equals to the `Type`, and in this particular case is `Classification` (defined [below](#type-definition-classification))

### Type definition Classification

```ts
interface Classification
 {	
  id: long;
  name: string;
  color: string;
  description: string;
  createdOn?: Date,
	updatedOn?: Date,
}
```

Example payload:

```json
  {
  
  "id": "123",
  "name": "Ejemplo Nombre de la clasificacion",
  "color": "rojo",
  "description": "Ejemplo de una descripcion",
  
}

```

:heavy_check_mark: 201 Ok Created
:x: 403 Unauthorized

# Clasification Functions

## Actions for ClassificationFunctions controller

| Verb   | URI                             | Action  | Route Name                                    | Response Type                  |
| ------ | ------------------------------- | ------- | --------------------------------------------- | ------------------------------ |
| GET    | `api/2/classifications`         | list    | `ClassificationFunctionsController#list()`    | [`ListWCursor<T>`](#responses) |
| GET    | `api/2/classifications/{id}`    | single  | `ClassificationFunctionsController#single()`  | `<T>`                          |


Here, `<T>` equals to the `Type`, and in this particular case is `ClassificationFunctionsDomain` (defined [below](#type-definition-classificationfunctions))


### Type definition ClassificationFunctions

```ts
interface ClassificationFunctions
 {	
  id: long;
  name: string;
  color: string;
  description: string;
  orden: int;
  createdOn?: Date,
	updatedOn?: Date,
}
```

Example payload:

```json
  {
  
  "id": "12233",
  "name": "Ejemplo Nombre de la clasificacion",
  "color": "rojo",
  "description": "Ejemplo de una descripcion",
  "orden": 1
  
}

```

:heavy_check_mark: 201 Ok Created
:x: 403 Unauthorized

# Coach Register

## Actions for CoachRegister controller

| Verb   | URI                                         | Action  | Route Name                          | Response Type                  |
| ------ | ------------------------------------------- | ------- | ----------------------------------- | ------------------------------ |
| GET    | `api/2/coachs-register`                     | list    | `CoachRegisterController#list()`    | [`ListWCursor<T>`](#responses) |
| GET    | `api/2/coachs-register/{id}`                | single  | `CoachRegisterController#single()`  | `<T>`                          |

Here, `<T>` equals to the `Type`, and in this particular case is `CoachRegister` (defined [below](#type-definition-coachregister))

### Type definition CoachRegister

```ts
interface CoachRegister
 {	
  address?: string;
  name: string;
  lastName: string;
  phone: string;
  dni: string;
  birthday?: string | Date;
  email?: string;
  namespace: string;
  status: 'requested' | 'accepted' | 'rejected';
  user?: string;
  pass?: string;
  pin: string;
  zone?: string;
  language?: string;
  createdOn?: Date,
	updatedOn?: Date,
}
```

Example payload:

```json
  {
  
  "address": "Call 12 av Principal",
  "name": "Jose",
  "lastName": "Perez",
  "phone": "1234567890",
  "dni": "A12345678",
  "birthday": "1990-01-05",
  "email": "joseP@gmail.com",
  "namespace": "JoseP",
  "status": "accepted",
  "user": "Jose Perez",
  "pass": "123",
  "pin": "1234",
  "zone": "ejemplo de zona",
  "language": "español"
  
}

```

:heavy_check_mark: 201 Ok Created
:x: 403 Unauthorized
:x: 422 DNI is required by new Coach
:x: 422 Name is required by new Coach
:x: 422 Phone is required by new Coach

# Competency Evaluation

## Actions for CompetencyEvaluation Controller

| Verb   | URI                                  | Action  | Route Name                                 | Response Type                  |
| ------ | ------------------------------------ | ------- | ------------------------------------------ | ------------------------------ |
| GET    | `api/2/evaluation-competencies`      | list    | `CompetencyEvaluationController#list()`    | [`ListWCursor<T>`](#responses) |
| GET    | `api/2/evaluation-competencies/{id}` | single  | `CompetencyEvaluationController#single()`  | `<T>`                          |

Here, `<T>` equals to the `Type`, and in this particular case is `CompetencyEvaluationDomain` (defined [below](#type-definition-competencyevaluation))

### Type definition CompetencyEvaluation

```ts
interface CompetencyEvaluation
 {	
  id: long;
  idAlumno: string;
  idCompetencia: string;
  idGrupo: string;
  idSesionProgramada: string;
  calificacion: float;
  createdOn?: Date,
	updatedOn?: Date,
}
```

Example payload:

```json
  {
  
  "id": "12324",
  "idAlumno": "123",
  "idCompetencia": "456",
  "idGrupo": "G789",
  "idSesionProgramada": "101",
  "calificacion": 85.5
  
}

```

:heavy_check_mark: 201 Ok Created
:x: 403 Unauthorized
:x: 422 Sesion is required by new Competency Evaluation Completed
:x: 422 Player is required by new Competency Evaluation Completed
:x: 422 Competency is required by new Competency Evaluation Completed

# Competency in Template

## Actions for CompetencyInTemplate Controller

| Verb   | URI                                   | Action  | Route Name                                 | Response Type                  |
| ------ | ------------------------------------- | ------- | ------------------------------------------ | ------------------------------ |
| GET    | `api/2/competencies-in-template`      | list    | `CompetencyInTemplateController#list()`    | [`ListWCursor<T>`](#responses) |
| GET    | `api/2/competencies-in-template/{id}` | single  | `CompetencyInTemplateController#single()`  | `<T>`                          |

Here, `<T>` equals to the `Type`, and in this particular case is `CompetencyInTemplate` (defined [below](#type-definition-competencyintemplate))

### Type definition CompetencyInTemplate

```ts
interface CompetencyInTemplate
 {	
  id: long;
  evaluationTemplateId: string;
  competencyId: string;
  competencyName: string;
  competencyKey: string;
  observablePatternIds: string[];
  observablePatterns?: ObPatternsDomain[];
  createdOn?: Date,
	updatedOn?: Date,
}
```

Example payload:

```json
 {
  "id": "1",
  "evaluationTemplateId": "ET123",
  "competencyId": "C456",
  "competencyName": "Skill Analysis",
  "competencyKey": "SA001",
  "observablePatternIds": ["1", "2", "3"],
  "observablePatterns": [
    {
    "idCompetency": 2345,
	  "nombre": "competenica correr",
	  "identif": "234",
	  "descripcion": "tecnica de correr",
	  "observaciones": "actitud",
    },
  
  ]
}

```

:heavy_check_mark: 201 Ok Created
:x: 403 Unauthorized
:x: 422 Not supported yet

# Config by  Microcycle

## Actions for ConfigByMicrocycle Controller

| Verb   | URI                               | Action  | Route Name                               | Response Type                  |
| ------ | --------------------------------- | ------- | ---------------------.....-------------- | ------------------------------ |
| GET    | `api/2/config-by-microcycle`      | list    | `ConfigByMicrocycleController#list()`    | [`ListWCursor<T>`](#responses) |
| GET    | `api/2/config-by-microcycle/{id}` | single  | `ConfigByMicrocycleController#single()`  | `<T>`                          |

Here, `<T>` equals to the `Type`, and in this particular case is `ConfigByMicrocycle` (defined [below](#type-definition-configbymicrocycle))

### Type definition ConfigByMicrocycle

```ts
interface ConfigByMicrocycle
 {	
  id: long;
  idMacro: string;
  idMeso: string;
  idMicro: string;  
  extraSessions: string[];
  createdOn?: Date,
	updatedOn?: Date,
}
```

Example payload:

```json
{
 "id": "1235",
  "idMacro": "123",
  "idMeso": "456",
  "idMicro": "789",
  "sessions": ["session1", "session2"]
}
}

```

:heavy_check_mark: 201 Ok Created
:x: 403 Unauthorized

# Config value

## Actions for ConfigValue Controller

| Verb   | URI                        | Action  | Route Name                        | Response Type                  |
| ------ | -------------------------- | ------- | --------------------------------- | ------------------------------ |
| GET    | `api/2/config-values`      | list    | `ConfigValueController#list()`    | [`ListWCursor<T>`](#responses) |
| GET    | `api/2/config-values/{id}` | single  | `ConfigValueController#single()`  | `<T>`                          |

Here, `<T>` equals to the `Type`, and in this particular case is `ConfigValue` (defined [below](#type-definition-configvalue))

### Type definition ConfigValue

```ts
interface ConfigValue
 {	
  id: long;
  type: string;
 
}
```

Example payload:

```json
{
 "id": "1234",
  "type": "EjemploType",
 
}

```

:heavy_check_mark: 201 Ok Created
:x: 403 Unauthorized
:x: 422 Type is required for Confg Value

# Convocatoria Info

## Actions for ConvocatoriaInfo Controller

| Verb   | URI                                                     | Action             | Route Name                                        | Response Type                           |
| ------ | ------------------------------------------------------- | ------------------ | ------------------------------------------------- | --------------------------------------- |
| GET    | `api/2/convocatoria-info`                               | list               | `ConvocatoriaInfoController#list()`               | [`ListWCursor<T>`](#responses)          |
| GET    | `api/2/convocatoria-info/{id}`                          | single             | `ConvocatoriaInfoController#single()`             | `<T>`                                   |
| GET    | `api/2/convocatoria-info/stars/{id}`                    | getStars           | `ConvocatoriaInfoController#getStars()`           | `{star_labels: string[], star_values: number[], convocatorias: T[]}`                                   |
| GET    | `api/2/convocatoria-info/by-player-comments-match/{id}` | commentsEvaluation | `ConvocatoriaInfoController#commentsEvaluation()` | `{entities: ArrayList<CommentByPlayer>}`|

Here, `<T>` equals to the `Type`, and in this particular case is `ConvocatoriaInfoDomain` (defined [below](#type-definition-convocatoriainfo))

### Type definition ConvocatoriaInfo

```ts
interface ConvocatoriaInfoDomain {
  id: long;
  idSesionProgramada: string;
  idTeam: string;
  idJugador: string;
  numeroJugador: int;
  comentario: string;
  observacionPositiva: string;
  observacionNegativa: string;
  calificacionStar: int;
  reason?: string; 
  createdOn?: Date,
	updatedOn?: Date,
}
```

Example payload:

```json
{
  "id": "123",
  "idSesionProgramada": "001",
  "idTeam": "456",
  "idJugador": "3489",
  "numeroJugador": 10,
  "comentario": "Buen rendimiento en la sesión.",
  "observacionPositiva": "Mostró gran liderazgo.",
  "observacionNegativa": "Necesita mejorar su resistencia.",
  "calificacionStar": 4,
  "reason": "Evaluación regular de la temporada"
{

```

:heavy_check_mark: 201 Ok Created
:x: 403 Unauthorized
:x: 422 Sesion is required by new ConvocatoriaInfot
:x: 422 Sesion is required by new ConvocatoriaInfo

# Dashboard Config

## Actions for DashboardConfig Controller

| Verb   | URI                              | Action  | Route Name                            | Response Type                  |
| ------ | -------------------------------- | ------- | ------------------------------------- | ------------------------------ |
| GET    | `api/2/dashboard-config`         | list    | `DashboardConfigController#list()`    | [`ListWCursor<T>`](#responses) |
| GET    | `api/2/dashboard-config/{id}`    | single  | `DashboardConfigController#single()`  | `<T>`                          |
| GET    | `api/2/dashboard-config/current` | current | `DashboardConfigController#current()` | [`ListWCursor<T>`]             |

Here, `<T>` equals to the `Type`, and in this particular case is `DashboardConfig` (defined [below](#type-definition-dashboardconfig))

### Type definition DashboardConfig

```ts
interface DashboardConfig {
  id: long;
  type: string; 
  reporting: string;
  status: string; 
  language: string; 
  createdOn?: Date,
	updatedOn?: Date,
}
```

Example payload:

```json
{
  "id": "12345",
  "type": "ejemploTipo",
  "reporting": "Detailes del reporte ",
  "status": "active",
  "language": "EN"
}



```

:heavy_check_mark: 201 Ok Created
:x: 403 Unauthorized
:x: 422 Type is required by new Dashboard Config

# Decision

## Actions for Decision Controller

| Verb   | URI                      | Action          | Route Name                            | Response Type                  |
| ------ | ------------------------ | --------------- | ------------------------------------- | ------------------------------ |
| GET    | `api/2/decision`         | list            | `DecisionController#list()`           | [`ListWCursor<T>`](#responses) |
| GET    | `api/2/decision/{id}`    | single          | `DecisionController#single()`         | `<T>`                          |
| GET    | `api/2/decision/restart` | restartDecision | `DecisionController#restartDecision)` | `String`                       |

Here, `<T>` equals to the `Type`, and in this particular case is `DecisionDomain` (defined [below](#type-definition-competition))

### Type definition Decision

```ts
interface Decision {
  id: long;
  decision: string;
  permissionSuper: boolean;
  createdOn?: Date,
	updatedOn?: Date,
}
```

Example payload:

```json
{
  "id": "7890",
  "decision": "Aprovada",
  "permissionSuper": false
}


```

:heavy_check_mark: 201 Ok Created
:x: 403 Unauthorized
:x: 422 Decision is required by new Decision

# Department

## Actions for Department Controller

| Verb   | URI                     | Action  | Route Name                          | Response Type                  |
| ------ | ----------------------- | ------- | ----------------------------------- | ------------------------------ |
| GET    | `api/2/department`      | list    | `DepartmentController#list()`       | [`ListWCursor<T>`](#responses) |
| GET    | `api/2/department/{id}` | single  | `DepartmentController#single()`     | `<T>`                          |

Here, `<T>` equals to the `Type`, and in this particular case is `DepartmentDomain` (defined [below](#type-definition-department))

### Type definition Department

```ts
interface Department {
  id: long;
  name: string;
  email: string;
  phone: string;
  createdOn?: Date,
	updatedOn?: Date,
}
```

Example payload:

```json
{
  "id": "7670",
  "name": "Valdevevas",
  "email": "Rmadrid.fc@gmail.com",
  "phone": "123456789"
}




```

:heavy_check_mark: 201 Ok Created
:x: 403 Unauthorized

# Document Name

## Actions for DocumentName Controller

| Verb   | URI                        | Action  | Route Name                         | Response Type                  |
| ------ | -------------------------- | ------- | ---------------------------------- | ------------------------------ |
| GET    | `api/2/document-name`      | list    | `DocumentNameController#list()`    | [`ListWCursor<T>`](#responses) |
| GET    | `api/2/document-name/{id}` | single  | `DocumentNameController#single()`  | `<T>`                          |

Here, `<T>` equals to the `Type`, and in this particular case is `DocumentName` (defined [below](#type-definition-documentname))

### Type definition DocumentName

```ts
interface DocumentName {
  id: long;
  name: string;
  description: string;
  status: string;
  idTypeDocument: string;
  canValidate: boolean;
  applyFor: int;
  createdOn?: Date,
	updatedOn?: Date,
}
```

Example payload:

```json
{
  "id": "4567",
  "name": "Passport",
  "description": "Descripcion del documento",
  "status": "active",
  "idTypeDocument": "1234564",
  "canValidate": true,
  "applyFor": 1
}





```

:heavy_check_mark: 201 Ok Created
:x: 403 Unauthorized
:X: 422 Short Name is mandatory
:X: 422 Description is mandatory
:X: 422 Type is mandatory

# Document type

## Actions for DocumentType Controller

| Verb   | URI                         | Action  | Route Name                         | Response Type                  |
| ------ | --------------------------- | ------- | ---------------------------------- | ------------------------------ |
| GET    | `api/2/document-types`      | list    | `DocumentTypeController#list()`    | [`ListWCursor<T>`](#responses) |
| GET    | `api/2/document-types/{id}` | single  | `DocumentTypeController#single()`  | `<T>`                          |

Here, `<T>` equals to the `Type`, and in this particular case is `DocumentType` (defined [below](#type-definition-documenttype))

### Type definition DocumentType

```ts
interface DocumentType {
  id: long;
  shortName: string;
  description: string;
  status: string;
  createdOn?: Date,
	updatedOn?: Date,
}
```

Example payload:

```json
{
  "id": "8910",
  "shortName": "Ejemplo de shorName",
  "description": "Ejemplo de una descrpcion",
  "status": "active"
}





```

:heavy_check_mark: 201 Ok Created
:x: 403 Unauthorized
:X: 422 Short Name is mandatory
:X: 422 Description is mandatory

# Document type sesion

## Actions for DocumentTypeSesion Controller

| Verb   | URI                               | Action  | Route Name                              | Response Type                  |
| ------ | ------------------- | ----------- | ------------------------------------------------- | ------------------------------ |
| GET    | `api/2/document-type-sesion`      | list    | `DocumentTypeSesionController#list()`   | [`ListWCursor<T>`](#responses) |
| GET    | `api/2/document-type-sesion/{id}` | single  | `DocumentTypeSesionController#single()` | `<T>`                          |

Here, `<T>` equals to the `Type`, and in this particular case is `DocumentTypeSesion` (defined [below](#type-definition-documenttypesesion))

### Type definition DocumentTypeSesion

```ts
interface DocumentTypeSesion {
  id: long;
  shortName: string;
  description: string;
  status: string;
  is_match: boolean;
  is_training: boolean;
  is_evaluation: boolean;
  createdOn?: Date,
	updatedOn?: Date,
}
```

Example payload:

```json
{
  "id": "2345",
  "shortName": "Session Plan",
  "description": "Ejemplo para una descripcion",
  "status": "active",
  "is_match": true,
  "is_training": false,
  "is_evaluation": true
}





```

:heavy_check_mark: 201 Ok Created
:x: 403 Unauthorized
:X: 422 Short Name is mandatory
:X: 422 Description is mandatory

# Enrollment Config

## Actions for EnrollmentConfig Controller

| Verb   | URI                                    | Action      | Route Name                                 | Response Type                  |
| ------ | -------------------------------------- | ----------- | ------------------------------------------ | ------------------------------ |
| GET    | `api/2/enrollment-configs`             | list        | `EnrollmentConfigController#list()`        | [`ListWCursor<T>`](#responses) |
| GET    | `api/2/enrollment-configs/{id}`        | single      | `EnrollmentConfigController#single()`      | `<T>`                          |

Here, `<T>` equals to the `Type`, and in this particular case is `EnrollmentConfig` (defined [below](#type-definition-enrollmentconfig))

### Type definition EnrollmentConfig

```ts
interface EnrollmentConfig {
  id: long;
  seasonId: string;
  teamId: string;
  enrolledPlayerIds: string[];
  enrolledPlayers: Player[]; 
  createdOn?: Date,
	updatedOn?: Date,
}
```

Example payload:

```json
{
  "id": "6789",
  "seasonId": "2021",
  "teamId": "89123",
  "enrolledPlayerIds": ["1232", "43452", "54643"],
  "enrolledPlayers": [
    {
      "id": "001",
      "firstName": "Jesus",
      "posicionJugador": "Delantero"
    },
    {
      "id": "002",
      "firstName": "Roberto",
      "posicionJugador": "Medio"
    },
    {
      "id": "003",
      "firstName": "Alex ",
      "posicionJugador": "Defensa"
    }
  ]
}


```

:heavy_check_mark: 201 Ok Created
:x: 403 Unauthorized
:X: 422 seasonId cannot be zero
:X: 422 teamId cannot be zero

# Evaluation Template

## Actions for EvaluationTemplate Controller

| Verb   | URI                               | Action  | Route Name                               | Response Type                  |
| ------ | --------------------------------- | ------- | ---------------------------------------- | ------------------------------ |
| GET    | `api/2/evaluation-templates`      | list    | `EvaluationTemplateController#list()`    | [`ListWCursor<T>`](#responses) |
| GET    | `api/2/evaluation-templates/{id}` | single  | `EvaluationTemplateController#single()`  | `<T>`                          |

Here, `<T>` equals to the `Type`, and in this particular case is `EvaluationTemplate` (defined [below](#type-definition-evaluationtemplate))

### Type definition EvaluationTemplate

```ts
  interface EvaluationTemplate {
  id: long;
  name: string;
  evaluationTemplateTypeId: string;
  competencies: CompetencyInTemplate[]; 
  createdOn?: Date,
	updatedOn?: Date,
}
```

Example payload:

```json
{
  "id": "3456",
  "name": "Evaluacion de Jugadores",
  "evaluationTemplateTypeId": "0234201",
  "competencies": [
    {
      "id": "001",     
    },
    {
      "id": "002"
    }
  ]
}



```

:heavy_check_mark: 201 Ok Created
:x: 403 Unauthorized
:X: 422 'name' cannot be null or empty
:X: 422 'evaluationTemplateTypeId' cannot be zero or lesser

# Evaluation Template Type

## Actions for EvaluationTemplateType Controller

| Verb   | URI                                    | Action | Route Name                                  | Response Type                  |
| ------ | -------------------------------------- | ------ | ------------------------------------------- | ------------------------------ |
| GET    | `api/2/evaluation-template-types`      | list   | `EvaluationTemplateTypeController#list()`   | [`ListWCursor<T>`](#responses) |
| GET    | `api/2/evaluation-template-types/{id}` | single | `EvaluationTemplateTypeController#single()` | `<T>`                          |

Here, `<T>` equals to the `Type`, and in this particular case is `EvaluationTemplateType` (defined [below](#type-definition-evaluationtemplatetype))

### Type definition EvaluationTemplateType

```ts
 interface EvaluationTemplateType {
  id: long;
  name: string;
  description: string;
  color: string;
  createdOn?: Date,
	updatedOn?: Date,
}
```

Example payload:

```json
{
  "id": "1234",
  "name": "Tecnicas Habilidosas",
  "description": "Tecnicas para mejorar las habilidades",
  "color": "rojo"
}




```

:heavy_check_mark: 201 Ok Created
:x: 403 Unauthorized

# Funcionalidades

## Actions for Funcionalidades Controller

| Verb   | URI                               | Action     | Route Name                               | Response Type                  |
| ------ | --------------------------------- | ---------- | ---------------------------------------- | ------------------------------ |
| GET    | `api/2/functionalities`           | list       | `FuncionalidadesController#list()`       | [`ListWCursor<T>`](#responses) |
| GET    | `api/2/functionalities/{id}`      | single     | `FuncionalidadesController#single()`     | `<T>`                          |
| GET    | `api/2/functionalities/listAll`   | listAll    | `FuncionalidadesController#listAll()`    | [`ListWCursor<T>`]             |

Here, `<T>` equals to the `Type`, and in this particular case is `FuncionalidadesDomain` (defined [below](#type-definition-funcionalidades))

### Type definition Funcionalidades

```ts
interface FuncionalidadesDomain {
  id: long;
  name: string;
  description: string;
  show: boolean;
  idClassification: string;
  classificationName: string;
  createdOn?: Date,
	updatedOn?: Date,
}
```

Example payload:

```json
{
  "id": "5678",
  "name": "Gestión de Usuarios",
  "description": "Gestionar cuentas de usuarios y permisos",
  "show": true,
  "idClassification": "435001",
  "classificationName": "Herramientas de Administración"
}

```

:heavy_check_mark: 201 Ok Created
:x: 403 Unauthorized

# Goal type

## Actions for GoalType Controller

| Verb   | URI                    | Action  | Route Name                     | Response Type                  |
| ------ | ---------------------- | ------- | ------------------------------ | ------------------------------ |
| GET    | `api/2/goal-type`      | list    | `GoalTypeController#list()`    | [`ListWCursor<T>`](#responses) |
| GET    | `api/2/goal-type/{id}` | single  | `GoalTypeController#single()`  | `<T>`                          |

Here, `<T>` equals to the `Type`, and in this particular case is `GoalTypeDomain` (defined [below](#type-definition-goaltype))

### Type definition GoalType

```ts
interface GoalTypeDomain {
  id: long;
  name: string;
  description: string;
  createdOn?: Date,
	updatedOn?: Date,
}
```

Example payload:

```json
{
  "id": "8901",
  "name": "Goles de entrenamiento",
  "description": "Goles realizados durante la practica"
}

```

:heavy_check_mark: 201 Ok Created
:x: 403 Unauthorized

# Line Up Node

## Actions for LineUpNode Controller

| Verb   | URI                      | Action  | Route Name                       | Response Type                  |
| ------ | ------------------------ | ------- | -------------------------------- | ------------------------------ |
| GET    | `api/2/lineup-node`      | list    | `LineUpNodeController#list()`    | [`ListWCursor<T>`](#responses) |
| GET    | `api/2/lineup-node/{id}` | single  | `LineUpNodeController#single()`  | `<T>`                          |

Here, `<T>` equals to the `Type`, and in this particular case is `LineUpNodeDomain` (defined [below](#type-definition-lineupnode))

### Type definition LineUpNode

```ts
interface LineUpNodeDomain {
  id: long;
  lineUpId: string;
  lineUp: string;
  sesionId: string;
  playerId: string;
  key: string;
  rival: boolean;
  player: Player;
  createdOn?: Date,
	updatedOn?: Date,
}
```

Example payload:

```json
{
  "id": "7890",
  "lineUpId": "3456",
  "lineUp": "Starting Line-Up",
  "sesionId": "354123",
  "playerId": "534789",
  "key": "A1",
  "rival": false,
  "player": {
    "id": "789",
    "firstName": "Paulo",
    "posicionJugador": "Prtero"
    
  }
}



```

:heavy_check_mark: 201 Ok Created
:x: 403 Unauthorized

# Line Up

## Actions for LineUp Controller

| Verb   | URI                  | Action | Route Name                   | Response Type                  |
| ------ | -------------------- | ------ | -----------------------------| ------------------------------ |
| GET    | `api/2/lineups`      | list   | `LineUpController#list()`    | [`ListWCursor<T>`](#responses) |
| GET    | `api/2/lineups/{id}` | single | `LineUpController#single()`  | `<T>`                          |

Here, `<T>` equals to the `Type`, and in this particular case is `LineUpDomain` (defined [below](#type-definition-lineup))

### Type definition LineUp

```ts
interface LineUpDomain {
  id: long;
  name: string;
  nodes: { [key: string]: LineUpNodeDomain };
  createdOn?: Date,
	updatedOn?: Date,
}
```

Example payload:

```json
{
  "id": "3456",
  "name": "Alineacion Titular",
   "nodes": {
    "node1": {
      "playerId":"9900",
      "sesionId":"8907",
    },
    "node2": {
      "playerId":"8979",
      "sesionId":"7890"
    }
  }
}


```

:heavy_check_mark: 201 Ok Created
:x: 403 Unauthorized

# Log Registry

## Actions for LogRegistry Controller

| Verb   | URI                      | Action  | Route Name                        | Response Type                  |
| ------ | ------------------------ | ------- | --------------------------------- | ------------------------------ |
| GET    | `api/2/logregistry`      | list    | `LogRegistryController#list()`    | [`ListWCursor<T>`](#responses) |
| GET    | `api/2/logregistry/{id}` | single  | `LogRegistryController#single()`  | `<T>`                          |

Here, `<T>` equals to the `Type`, and in this particular case is `LogRegistryDomain` (defined [below](#type-definition-logregistry))

### Type definition LogRegistry

```ts
interface LogRegistry {
  id: long;
  origin: string;
  error: string;
  empresa: string;
  createdOn?: Date,
	updatedOn?: Date,
}

```

Example payload:

```json
{
  "id": "0534501",
  "origin": "Ejemplo de origen",
  "error": "Algun tipo de error",
  "empresa": "UA."
}




```

:heavy_check_mark: 201 Ok Created
:x: 403 Unauthorized
:x: 422 Origin is required by new Sesion

# Match

## Actions for Match Controller

| Verb   | URI                        | Action     | Route Name                     | Response Type                  |
| ------ | -------------------------- | ---------- | ------------------------------ | ------------------------------ |
| GET    | `api/2/match`              | list       | `MatchController#list()`       | [`ListWCursor<T>`](#responses) |
| GET    | `api/2/match/{id}`         | single     | `MatchController#single()`     | `<T>`                          |
| GET    | `api/2/match/{id}/players` | getPlayers | `MatchController#getPlayers()` | [`List<Player>`]               |
| GET    | `api/2/match/call/{id}`    | call       | `MatchController#call()`       | [`{logo: string, scheduledSession: ScheduledSession,match: Match, players: List<Player>, call: List<ConvocatoriaInfo>}`] |

Here, `<T>` equals to the `Type`, and in this particular case is `MatchDomain` (defined [below](#type-definition-match))

### Type definition Match

```ts
interface Match {
  id: long;
  idSesionProgramada: string;
  idSesion: string;
  lineUpId: string;
  lineUps: { [key: string]: string };
  lineUpIdRival: string;
  titulares: string[];
  suplentes: string[];
  convocados: string[];
  noConvocados: string[];
  agregados: string[];
  golAFavor: string;
  golEnContra: string;
  obsDefensaPositiva: string;
  obsDefensaMejorar: string;
  obsAtaquePositiva: string;
  obsAtaqueMejorar: string;
  obsBalonParadoPositiva: string;
  obsBalonParadoMejorar: string;
  obsGenerales: string;
  isShootout: boolean;
  isPenalty: boolean;
  isExtraTime: boolean;
  isMatchLight: boolean;
  numberExtraTime: int;
  minutesExtraTime: int;
  playerProperties: { [key: string]: string[] }; 
  lineUp: LineUpDomain; 
  rival: string; 
  arbitro: string; 
  createdOn?: Date,
	updatedOn?: Date,
}

```

Example payload:

```json
{
  "id": "112001",
  "idSesionProgramada": "123",
  "idSesion": "56",
  "lineUpId": "789",
  "lineUps": {
  "id": "7890",
  "lineUpId": "3456",
  "lineUp": "Inicial",
  },
  "lineUpIdRival": "234456",
  "titulares": ["jugador1", "jugador2", "jugador3"],
  "suplentes": ["jugador4", "jugador5"],
  "convocados": ["jugador1", "jugador2", "jugador3", "jugador4", "jugador5"],
  "noConvocados": ["jugador6", "jugador7"],
  "agregados": ["jugador8"],
  "golAFavor": "3",
  "golEnContra": "2",
  "obsDefensaPositiva": "Buena actuación defensiva",
  "obsDefensaMejorar": "Mejorar el marcaje",
  "obsAtaquePositiva": "Ataque efectivo",
  "obsAtaqueMejorar": "Mejorar la finalización",
  "obsBalonParadoPositiva": "Bien ejecutadas las jugadas a balón parado",
  "obsBalonParadoMejorar": "Más práctica necesaria",
  "obsGenerales": "Desempeño general bueno",
  "isShootout": true,
  "isPenalty": false,
  "isExtraTime": true,
  "isMatchLight": false,
  "numberExtraTime": 2,
  "minutesExtraTime": 30,
  "playerProperties": {
    "jugador1": ["velocidad", "resistencia"],
    "jugador2": ["fuerza", "habilidad"]
  },
  "lineUp": {
    "id": "8789",
    "name": "Alineación Inicial",
    "nodes": {
      "nodo1": {
      "playerId":"9900",
      "sesionId":"8907",
      }
    }
  },
  "rival": "Equipo B",
  "arbitro": "Árbitro X",
  "season": "2024"
}





```

:heavy_check_mark: 201 Ok Created
:x: 403 Unauthorized
:x: 422 Sesion is required by new Match
:x: 422 LineUp Key is not a valid key

# Match Event

## Actions for MatchEvent Controller

| Verb   | URI                               | Action                 | Route Name                                      | Response Type                  |
| ------ | --------------------------------- | ---------------------- | ----------------------------------------------- | ------------------------------ |
| GET    | `api/2/match-events`              | list                   | `MatchEventController#list()`                   | [`ListWCursor<T>`](#responses) |
| GET    | `api/2/match-events/{id}`         | single                 | `MatchEventController#single()`                 | `<T>`                          |
| GET    | `api/2/match-events/graph`        | graphImage             | `MatchEventController#graphImage()`             | `String`                       |

Here, `<T>` equals to the `Type`, and in this particular case is `MatchEventDomain` (defined [below](#type-definition-matchevent))

### Type definition MatchEvent

```ts
interface MatchEvent {
  id: long;
  idCompoused: string;
  scheduledSessionId: string;
  playerId: string;
  outPlayerId: string;
  goalTypeId: string;
  event: string;
  type: string;
  min: int;
  time: string;
  comment: string;
  seasonId: string;
  scheduledSession?: SesionProgramadaDomain; 
  player: Player; 
  outPlayer: Player; 
  season: TemporadaDomain; 
  createdOn?: Date,
	updatedOn?: Date,
}

```

Example payload:

```json
{
  "id": "234001",
  "idCompoused": "65001",
  "scheduledSessionId": "65001",
  "playerId": "player001",
  "outPlayerId": "546502",
  "goalTypeId": "656001",
  "event": "goal",
  "type": "official",
  "min": 25,
  "time": "00:25",
  "comment": "Gol de gran calidad desde fuera del área",
  "seasonId": "2024",
  "scheduledSession": {
    "id": "234001",
    "tipoSesion": "ejemploTipoSesion",
    "realizado": "true"  
  },
  "player": {
    "id": "00154",
    "firstName": "Juan ",
    "posicionJugador": "Delantero"
  },
  "outPlayer": {
    "id": "5601",
    "firstName": "Marco",
    "posicionJugador": "Delantero"
  },
  "season": {
    "id": "2024",
    "nombre": "Temporada 2024",
  }
}

```

:heavy_check_mark: 201 Ok Created
:x: 403 Unauthorized

# Match Template

## Actions for MatchTemplate Controller

| Verb   | URI                          | Action  | Route Name                          | Response Type                  |
| ------ | ---------------------------- | ------- | ----------------------------------- | ------------------------------ |
| GET    | `api/2/match-templates`      | list    | `MatchTemplateController#list()`    | [`ListWCursor<T>`](#responses) |
| GET    | `api/2/match-templates/{id}` | single  | `MatchTemplateController#single()`  | `<T>`                          |

Here, `<T>` equals to the `Type`, and in this particular case is `MatchTemplate` (defined [below](#type-definition-matchtemplate))

### Type definition MatchTemplate

```ts
interface MatchTemplate {
  id: long;
  name: string;
  description: string;
  stages: int;
  timePerStage: int;
  gameSystems: string[];
  defaultTemplate: boolean;
  sportName: string;
  sportImage: string;
  numPlayers: int;
  createdOn?: Date,
	updatedOn?: Date,
}

```

Example payload:

```json
{
  "id": "889001",
  "name": "Template A",
  "description": "Descripción del template A",
  "stages": 3,
  "timePerStage": 20,
  "gameSystems": ["4-3-3", "4-4-2"],
  "defaultTemplate": true,
  "sportName": "Fútbol",
  "sportImage": "url/to/sport-image.jpg",
  "numPlayers": 11
}


```

:heavy_check_mark: 201 Ok Created
:x: 403 Unauthorized

# News

## Actions for News Controller

| Verb   | URI                   | Action  | Route Name                 | Response Type                   |
| ------ | --------------------- | ------- | -------------------------- | ------------------------------  |
| GET    | `api/2/news`         | list    | `NewsController#list()`     | [`ListWCursor<T>`](#responses)  |
| GET    | `api/2/news/{id}`    | single  | `NewsController#single()`   | `<T>`                           |

Here, `<T>` equals to the `Type`, and in this particular case is `News` (defined [below](#type-definition-news))

### Type definition News

```ts
interface News {
  id: long;
  title: string;
  shortDescription: string;
  content: string;
  pageWeb: string;
  image: string;
  newVersion: boolean;
  createdOn?: Date,
	updatedOn?: Date,
}

```

Example payload:

```json
{
  "id": "423401",
  "title": "Título de la Noticia",
  "shortDescription": "Descripción breve de la noticia",
  "content": "Contenido completo de la noticia.",
  "pageWeb": "https://DiarioelClarin.com/noticia",
  "image": "https://Daily.com/imagen.jpg",
  "newVersion": true
}


```

:heavy_check_mark: 201 Ok Created
:x: 403 Unauthorized

# Notification

## Actions for Notification Controller

| Verb   | URI                                    | Action          | Route Name                                 | Response Type                     |
| ------ | -------------------------------------- | --------------- | ------------------------------------------ | --------------------------------- |
| GET    | `api/2/notifications`                  | list            | `NotificationController#list()`            | [`ListWCursor<T>`](#responses)    |
| GET    | `api/2/notifications/{id}`             | single          | `NotificationController#single()`          | `<T>`                             |
| GET    | `/my-notification/{userId}`            | myNotification  | `NotificationController#myNotification()`  | [`ListWCursor<UserNotification>`] |
| GET    | `/my-notification/unassign/{platform}` | unassign        | `NotificationController#unassign()`        | `String`                          |

Here, `<T>` equals to the `Type`, and in this particular case is `WebNotification` (defined [below](#type-definition-notification))

### Type definition Notification

```ts
interface WebNotification {
  id: long;
  notificationId: string;
  notification: Notification; 
  topic: string; 
  token: string; 
  condition: string[]; 
  senderId: string;
  usersId: string[];
  createdOn?: Date,
	updatedOn?: Date,
}

```

Example payload:

```json
{
  "id": "645601",
  "notificationId": "654123",
  "notification": {
    "title": "Notificación Importante",
    "body": "Este es un mensaje de notificación."
  },
  "topic": "news_updates",
  "token": "userToken123",
  "condition": ["condicion1", "condicion2"],
  "senderId": "9092101",
  "usersId": ["1865", "4352", "21433"]
}



```

:heavy_check_mark: 201 Ok Created
:x: 403 Unauthorized

# ObPattern Evaluado

## Actions for ObPatternEvaluado Controller

| Verb   | URI                           | Action  | Route Name                              | Response Type                  |
| ------ | ------------------------------| ------- | ----------------------------------------| ------------------------------ |
| GET    | `/api/2/obpattern-eval`       | list    | `ObPatternEvaluadoController#list()`    | [`ListWCursor<T>`](#responses) |
| GET    | `/api/2/obpattern-eval/{id}`  | single  | `ObPatternEvaluadoController#single()`  | `<T>`                          |

Here, `<T>` equals to the `Type`, and in this particular case is `ObPatternEvaluado` (defined [below](#type-definition-obpatternevaluado))

### Type definition ObPatternEvaluado

```ts
interface ObPatternEvaluado {
  id: long;
  idObPattern: string;
  type: "check" | "likert" | "scale";
  calificacion: float;
  obPatternName: string; 
  obPatternDescription: string;
  createdOn?: Date,
	updatedOn?: Date,
}

```

Example payload:

```json
{
  "id": "54654",
  "idObPattern": "45665",
  "type": "scale",
  "calificacion": 8.5,
  "obPatternName": "Patrón Observado A",
  "obPatternDescription": "Descripción detallada del patrón observado A."
}




```

:heavy_check_mark: 201 Ok Created
:x: 403 Unauthorized
:x: 422 ObPattern is required by new ObPatternEval

# Observation

## Actions for Observation Controller

| Verb   | URI                       | Action  | Route Name                          | Response Type                  |
| ------ | --------------------------| ------- | ----------------------------------- | ------------------------------ |
| GET    | `/api/2/observation`      | list    | `ObservationController#list()`      | [`ListWCursor<T>`](#responses) |
| GET    | `/api/2/observation/{id}` | single  | `ObservationController#single()`    | `<T>`                          |

Here, `<T>` equals to the `Type`, and in this particular case is `ObservationDomain` (defined [below](#type-definition-observation))

### Type definition Observation

```ts
interface Observations {
  id: long;
  comment: string;
  idPlayer: string;
  createdOn?: Date,
	updatedOn?: Date,
}

```

Example payload:

```json
{
  "id": "435501",
  "comment": "Observación sobre el desempeño del jugador.",
  "idPlayer": "56123"
}





```

:heavy_check_mark: 201 Ok Created
:x: 403 Unauthorized
:x: 422 Player is required by new observation
:x: 422 Comment is required by new observation

# Opponent Player

## Actions for OpponentPlayer Controller

| Verb   | URI                       | Action       | Route Name                              | Response Type                  |
| ------ | ------------------------- | -------------| --------------------------------------- | ------------------------------ |
| GET    | `/api/2/opponents`        | list         | `OpponentPlayerController#list()`       | [`ListWCursor<T>`](#responses) |
| GET    | `/api/2/opponents/{id}`   | single       | `OpponentPlayerController#single()`     | `<T>`                          |

Here, `<T>` equals to the `Type`, and in this particular case is `OpponentPlayer` (defined [below](#type-definition-opponentplayer))

### Type definition OpponentPlayer

```ts
interface OpponentPlayer {
  id: long;
  parentSession: string;
  name: string;
  number: string;
  parent: SesionProgramadaDomain; 
  createdOn?: Date,
	updatedOn?: Date,
}

```

Example payload:

```json
{
  "id": "89301",
  "parentSession": "230423",
  "name": "Jugador Oponente",
  "number": "10",
  "parent": {
    "id": "23423",
    "lugar":"Campo de Entranamiento"
  }
}






```

:heavy_check_mark: 201 Ok Created
:x: 403 Unauthorized

# Plan And Session Classification

## Actions for PlanAndSessionClassification Controller

| Verb   | URI                                                   | Action          | Route Name                                                | Response Type                  |
| ------ | ------------------------------------------------------|-----------------|-----------------------------------------------------------| ------------------------------ |
| GET    | `/api/2/plan-and-session-classifications`             | list            | `PlanAndSessionClassificationController#list()`           | [`ListWCursor<T>`](#responses) |
| GET    | `/api/2/plan-and-session-classifications/{id}`        | single          | `PlanAndSessionClassificationController#single()`         | `<T>`                          |
| GET    | `/api/2/plan-and-session-classifications/plan-session`| getPlanSession  | `PlanAndSessionClassificationController#getPlanSession()` | `PlanSessionLandingData`       |

Here, `<T>` equals to the `Type`, and in this particular case is `PlanAndSessionClassification` (defined [below](#type-definition-planandsessionclassification))

### Type definition PlanAndSessionClassification

```ts
interface PlanAndSessionClassification {
  id: long;
  name: string;
  description: string;
  createdOn?: Date,
	updatedOn?: Date,
}


```

Example payload:

```json
{
  "id": "3241",
  "name": "Clasificación de Plan y Sesión",
  "description": "Descripción detallada de la clasificación de plan y sesión."
}



```

:heavy_check_mark: 201 Ok Created
:x: 403 Unauthorized

# Plan Content Loaded

## Actions for PlanContentLoaded Controller

| Verb   | URI                               | Action  | Route Name                              | Response Type                  |
| ------ | --------------------------------- | ------- | --------------------------------------- | ------------------------------ |
| GET    | `/api/2/plan-content-loaded`      | list    | `PlanContentLoadedController#list()`    | [`ListWCursor<T>`](#responses) |
| GET    | `/api/2/plan-content-loaded/{id}` | single  | `PlanContentLoadedController#single()`  | `<T>`                          |

Here, `<T>` equals to the `Type`, and in this particular case is `PlanContentLoaded` (defined [below](#type-definition-plancontentloaded))

### Type definition PlanContentLoaded

```ts
interface PlanContentLoaded {
  id: long;
  seasonId: string;
  planId: string;
  teamId: string;
  startDate: string; 
  type: 'whole-plan' | 'only-micro' | 'start-end-micro';
  startMacroId: string;
  startMesoId: string;
  startMicroId: string;
  endMacroId: string;
  endMesoId: string;
  endMicroId: string;
  timezoneOffset: int; 
  trainingSchedules: { [key: string]: string[] }; 
  evaluationSchedules: { [key: string]: string[] }; 
  sessionSchedules: { [key: string]: string }; 
  usedMicrocycleIds: string[]; 
  location: string;
  createdOn?: Date,
	updatedOn?: Date,
}



```

Example payload:

```json
{
  "id": "8983",
  "seasonId": "2024",
  "planId": "12456",
  "teamId": "12789",
  "startDate": "2024-08-11",
  "type": "whole-plan",
  "startMacroId": "123",
  "startMesoId": "1235",
  "startMicroId": "12903",
  "endMacroId": "43523",
  "endMesoId": "324123",
  "endMicroId": "43123",
  "timezoneOffset": 120,
  "trainingSchedules": {
    "Monday": ["09:00", "14:00"],
    "Wednesday": ["10:00", "15:00"]
  },
  "evaluationSchedules": {
    "Friday": ["11:00", "16:00"]
  },
  "sessionSchedules": {
    "type": "only-micro", //'whole-plan' | 'only-micro' | 'start-end-micro' are acceptable for field 'type
  },
  "usedMicrocycleIds": ["34456", "5657"],
  "location": "Campo de Entrenamiento A"
}




```

:heavy_check_mark: 201 Ok Created
:x: 403 Unauthorized
:x: 422 'seasonId' cannot be 0 or lesser
:x: 422 'planId' cannot be 0 or lesser
:x: 422 'teamId' cannot be 0 or lesser
:x: 422 At least one of 'sessionSchedules' or 'trainingSchedules' must be present

# Plan Schedule

## Actions for PlanScheduler Controller

| Verb   | URI                          | Action           | Route Name                                   | Response Type                  |
| ------ | ---------------------------- | ---------------- | ---------------------------------------------| ------------------------------ |
| GET    | `/api/2/plan-scheduler`      | list             | `PlanSchedulerController#list()`             | [`ListWCursor<T>`](#responses) |
| GET    | `/api/2/plan-scheduler/{id}` | single           | `PlanSchedulerController#single()`           | `<T>`                          |

Here, `<T>` equals to the `Type`, and in this particular case is `PlanScheduler` (defined [below](#type-definition-planscheduler))

### Type definition PlanScheduler

```ts


interface PlanScheduler {
  training: string; 
  evaluation: string; 
  maxMinDuration: int; 
  tSchedules: HorariosDomain[]; 
  eSchedules: HorariosDomain[]; 
  tScheduleIndex: int; 
  eScheduleIndex: int; 
  content: PlanContentLoaded; 
  lastMicroIdOfTSesion: string; 
  lastDayOfTSesion: int; 
  lastMicroIdOfESesion: string; 
  lastDayOfESesion: int; 
  timezoneOffset: int; 
  firstEDay: int; 
  createdOn?: Date,
	updatedOn?: Date,
}




```

Example payload:

```json
{
  "training": "2024-08-10",
  "evaluation": "2024-08-20",
  "maxMinDuration": 90,
  "tSchedules": [
    {
      "id": "001",
      "idGrupo": "1",
      "dia": 1,
      "horaIni": "09:00",
      "horaFin": "10:30"
    }
  ],
  "eSchedules": [
    {
      "id": "002",
      "idGrupo": "2",
      "dia": 5,
      "horaIni": "10:00",
      "horaFin": "11:30"
    }
  ],
  "tScheduleIndex": 0,
  "eScheduleIndex": 1,
  "content": {
    "id": "34301",
    "seasonId": "2023",
    "planId": "6756",
    "teamId": "789",
    "startDate": "2024-08-10",
    "type": "whole-plan",
    "startMacroId": "1823",
    "startMesoId": "1923",
    "startMicroId": "1723",
    "endMacroId": "4123",
    "endMesoId": "7123",
    "endMicroId": "2323",
    "timezoneOffset": 120,
    "trainingSchedules": {
      "Monday": ["09:00", "14:00"],
      "Wednesday": ["10:00", "15:00"]
    },
    "evaluationSchedules": {
      "Friday": ["11:00", "16:00"]
    },
    "sessionSchedules": {
      "type": "only-micro", //'whole-plan' | 'only-micro' | 'start-end-micro' are acceptable for field 'type
    },
    "usedMicrocycleIds": ["microcycle1", "microcycle2"],
    "location": "Campo de Entrenamiento A"
  },
  "lastMicroIdOfTSesion": "76123",
  "lastDayOfTSesion": 15,
  "lastMicroIdOfESesion": "687456",
  "lastDayOfESesion": 20,
  "timezoneOffset": 120,
  "firstEDay": 1
}


```

:heavy_check_mark: 201 Ok Created
:x: 403 Unauthorized

# Player Request

## Actions for PlayerRequest Controller

| Verb   | URI                                        | Action         | Route Name                                 | Response Type                  |
| ------ | ------------------------------------------ | -------------- | ------------------------------------------ | ------------------------------ |
| GET    | `/api/2/player-request`                    | list           | `PlayerRequestController#list()`           | [`ListWCursor<T>`](#responses) |
| GET    | `/api/2/player-request/{id}`               | single         | `PlayerRequestController#single()`         | `<T>`                          |

Here, `<T>` equals to the `Type`, and in this particular case is `PlayerRequest` (defined [below](#type-definition-playerrequest))

### Type definition PlayerRequest

```ts


interface PlayerRequest {
  id: long;
  userPetition: string;
  userPetitionDomain?: UserDomain;
  idKaantera: string;
  firstName: string;
  lastName: string;
  birthDate: string; 
  status: int;
  createdOn?: Date,
	updatedOn?: Date,
}






```

Example payload:

```json
{
  "id": "324321",
  "userPetition": "petition123",
  "userPetitionDomain": {
    "id": "2343",
    "nombre": "Ana",
    "lastName": "Gomez",
  },
  "idKaantera": "456",
  "firstName": "Ana",
  "lastName": "Gomez",
  "birthDate": "2000-01-01",
  "status": 1
}

```

:heavy_check_mark: 201 Ok Created
:x: 403 Unauthorized

# Player Time By Match

## Actions for PlayerTimeByMatch Controller

| Verb   | URI                               | Action  | Route Name                              | Response Type                  |
| ------ | --------------------------------- | ------- | --------------------------------------- | ------------------------------ |
| GET    | `/api/2/player-time-by-match`     | list    | `PlayerTimeByMatchController#list()`    | [`ListWCursor<T>`](#responses) |
| GET    | `/api/2/player-time-by-match/{id}`| single  | `PlayerTimeByMatchController#single()`  | `<T>`                          |


Here, `<T>` equals to the `Type`, and in this particular case is `PlayerTimeByMatch` (defined [below](#type-definition-playertimebymatch))

### Type definition PlayerTimeByMatch

```ts


interface PlayerTimeByMatch {
  id: long;
  playerId: string;
  teamId: string;
  seasonId: string;
  scheduledSessionId: string;
  sessionId: string;
  matchId: string;
  createdOn?: Date,
	updatedOn?: Date,
}






```

Example payload:

```json
{
  "id": "12001",
  "playerId": "12323",
  "teamId": "1236",
  "seasonId": "80989",
  "scheduledSessionId": "12301",
  "sessionId": "12302",
  "matchId": "12303"
}

```

:heavy_check_mark: 201 Ok Created
:x: 403 Unauthorized
:x: 422 Match is required by new PlayerTimeByMatch
:x: 422 Player is required by new PlayerTimeByMatch
:x: 422 Season is required by new PlayerTimeByMatch
:x: 422 ScheduledSession is required by new PlayerTimeByMatch
:x: 422 Session is required by new PlayerTimeByMatch
:x: 422 Team is required by new PlayerTimeByMatch

# Principle

## Actions for Principle Controller

| Verb   | URI                        | Action  | Route Name                      | Response Type                  |
| ------ | ---------------------------| ------- | --------------------------------| ------------------------------ |
| GET    | `/api/2/principles`        | list    | `PrincipleController#list()`    | [`ListWCursor<T>`](#responses) |
| GET    | `/api/2/principles/{id}`   | single  | `PrincipleController#single()`  | `<T>`                          |

Here, `<T>` equals to the `Type`, and in this particular case is `Principle` (defined [below](#type-definition-principle))

### Type definition Principle

```ts


interface Principle {
  id: long;
  name: string;
  description: string;
  classificationId: string;
  classification: Classification;
  createdOn?: Date,
	updatedOn?: Date,
}



```

Example payload:

```json
{
  "id": "12301",
  "name": "Principios Defensivos",
  "description": "ejemplo detallado de la descripcion.",
  "classificationId": "12431",
  "classification": {
    "id": "3421",
    "name": "Ofensividad",
    "color": "rojo",
    "description": "Prinicipios detallados con estrategias y puntos basicos."
  }
}


```

:heavy_check_mark: 201 Ok Created
:x: 403 Unauthorized

# Profile

## Actions for Profile Controller

| Verb   | URI                    | Action  | Route Name                    | Response Type                  |
| ------ | -----------------------| ------- | ------------------------------| ------------------------------ |
| GET    | `/api/2/profiles`      | list    | `ProfileController#list()`    | [`ListWCursor<T>`](#responses) |
| GET    | `/api/2/profiles/{id}` | single  | `ProfileController#single()`  | `<T>`                          |


Here, `<T>` equals to the `Type`, and in this particular case is `ProfileDomain` (defined [below](#type-definition-profile))

### Type definition Profile

```ts


interface Profile {
  id: long;
  descripcion: string;
  orden: int;
  createdOn?: Date,
	updatedOn?: Date,
}



```

Example payload:

```json
{
  "id": "101201",
  "descripcion": "Perfil de Entrenador",
  "orden": 1
}


```

:heavy_check_mark: 201 Ok Created
:x: 403 Unauthorized

# Publicacion

## Actions for Publicacion Controller

| Verb   | URI                       | Action  | Route Name                        | Response Type                  |
| ------ | --------------------------| ------- | ----------------------------------| ------------------------------ |
| GET    | `/api/2/publicacion`      | list    | `PublicacionController#list()`    | [`ListWCursor<T>`](#responses) |
| GET    | `/api/2/publicacion/{id}` | single  | `PublicacionController#single()`  | `<T>`                          |


Here, `<T>` equals to the `Type`, and in this particular case is `Publicacion` (defined [below](#type-definition-publicacion))

### Type definition Publicacion

```ts


interface Publicacion {
  id: long;
  titulo: string;
  contenido: string;
  paginaWeb: string;
  temaId: string;
  base64: string;
  newVersion: boolean;
  createdOn?: Date,
	updatedOn?: Date,
}



```

Example payload:

```json
{
  "id": "8993",
  "titulo": "Actualización de la Plataforma",
  "contenido": "Se han implementado nuevas funcionalidades en la plataforma.",
  "paginaWeb": "https://www.ejemplo.com",
  "temaId": "12323",
  "base64": "dGVzdAjhadsid8a9sdha9a0",
  "newVersion": true
}



```

:heavy_check_mark: 201 Ok Created
:x: 403 Unauthorized

# Reason Non Attendance

## Actions for ReasonNonAttendance Controller

| Verb   | URI                                 | Action  | Route Name                                | Response Type                  |
| ------ | ------------------------------------| ------- | ------------------------------------------| ------------------------------ |
| GET    | `/api/2/reason-non-attendance`      | list    | `ReasonNonAttendanceController#list()`    | [`ListWCursor<T>`](#responses) |
| GET    | `/api/2/reason-non-attendance/{id}` | single  | `ReasonNonAttendanceController#single()`  | `<T>`                          |

Here, `<T>` equals to the `Type`, and in this particular case is `ReasonNonAttendance` (defined [below](#type-definition-reasonnonattendance))

### Type definition ReasonNonAttendance

```ts


interface ReasonNonAttendance {
  id: long;
  description: string;
  createdOn?: Date,
	updatedOn?: Date,
}



```

Example payload:

```json
{
  "id": "123243",
  "description": "Motivo de no asistencia"
}




```

:heavy_check_mark: 201 Ok Created
:x: 403 Unauthorized
:x: 422 Description is required by new Reason Non Attendance

# Schedule

## Actions for Schedule Controller

| Verb   | URI                    | Action  | Route Name                          | Response Type                  |
| ------ | ---------------------- | ------- | ----------------------------------- | ------------------------------ |
| GET    | `/api/2/schedules`     | list    | `ScheduleController#list()`         | [`ListWCursor<T>`](#responses) |
| GET    | `/api/2/schedules/{id}`| single  | `ScheduleController#single()`       | `<T>`                          |

Here, `<T>` equals to the `Type`, and in this particular case is `HorariosDomain` (defined [below](#type-definition-schedule))

### Type definition Schedule

```ts


interface Horarios {
  id: long;
  idGrupo: string;
  dia: int;
  horaIni: string;
  horaFin: string;
  createdOn?: Date,
	updatedOn?: Date,
}



```

Example payload:

```json
{
  "id": "012345",
  "idGrupo": "1777",
  "dia": 2,
  "horaIni": "08:00",
  "horaFin": "10:00"
}




```

:heavy_check_mark: 201 Ok Created
:x: 403 Unauthorized
:x: 422 Team is required by new Schedule
:x: 422 Day is required by new Schedule
:x: 422 Init hour is required by new Schedule
:x: 422 End hour is required by new Schedule

# Scheduled Session

## Actions for ScheduledSession Controller

| Verb   | URI                                            | Action                       | Route Name                                                  | Response Type                  |
| ------ | -----------------------------------------------| -----------------------------| ------------------------------------------------------------|--------------------------------|
| GET    | `/api/2/scheduled-sessions`                    | list                         | `ScheduledSessionController#list()`                         | [`ListWCursor<T>`](#responses) |
| GET    | `/api/2/scheduled-sessions/{id}`               | single                       | `ScheduledSessionController#single()`                       | `<T>`                          |

Here, `<T>` equals to the `Type`, and in this particular case is `ScheduledSession` (defined [below](#type-definition-scheduledsession))

### Type definition ScheduledSession

```ts


interface SesionProgramadaDomain {
  id: long;
  fechaIni: string; 
  fechaFin: string; 
  fechaIniVirtual: int; 
  fechaFinVirtual: int; 
  teamid: string;
  sesionid: string;
  coachid: string;
  idTemporada: string;
  tipoSesion: string;
  status: string;
  realizado: boolean;
  sesionEspecial: boolean;
  idCustomBlocked: string;
  players: string[];
  tasks: int[];
  obPatterns: int[];
  extraSessionObjId: string[];
  extraTasksId: string[];
  lugar: string;
  sede: string;
  extraCompetenciesId: string[];
  extraTasksIdBySection: { [key: string]: string[] };
  usedExtraCompetencies: { [key: string]: boolean };
  fromPlan: boolean;
  temporada: TemporadaDomain; 
  team: GrupoDomain; 
  sesion: SesionDomain; 
  extraCompetencies?: CompetenciesDomain[]; 
  matchTemplateId: string; 
  matchTemplate: MatchTemplate; 
  createdOn?: Date,
	updatedOn?: Date,
}



```

Example payload:

```json
{
  "id": "23432",
  "fechaIni": "2024-08-09",
  "fechaFin": "2024-08-19",
  "fechaIniVirtual": 1724368800,
  "fechaFinVirtual": 1724376000,
  "teamid": "6523",
  "sesionid": "65356",
  "coachid": "4789",
  "idTemporada": "2024",
  "tipoSesion": "cardio",
  "status": "activo",
  "realizado": false,
  "sesionEspecial": true,
  "idCustomBlocked": "3401",
  "players": ["player1", "player2", "player3"],
  "tasks": [1001, 1002],
  "obPatterns": [2001, 2002],
  "extraSessionObjId": ["4321", "4234"],
  "extraTasksId": ["3451", "3452"],
  "lugar": "Campo A",
  "sede": "Camp Nou B",
  "extraCompetenciesId": ["2311", "6542"],
  "extraTasksIdBySection": {
    "section1": ["123", "3123"],
    "section2": ["544563"]
  },
  "usedExtraCompetencies": {
    "comp1": true,
    "comp2": false
  },
  "fromPlan": true,
  "temporada": {
    "id": "2024",
    "nombre": "Season 2024"
  },
  "team": {
  "id": "234123",
  "nombre": "Nombre05",
  "descripcion": "Descripción del equipo",
  "observaciones": "Observaciones adicionales sobre el equipo",
  },
  "sesion": {
    "id": "12456",
    "name": "Session 1"
  },
  "extraCompetencies": [
    {
      "id": "1231",
      "name": "Competency 1"
    }
  ],
  "matchTemplateId": "1234401",
  "matchTemplate": {
  "id": "889001",
  "name": "Template A",
  "description": "Descripción del template A",
  }
}



```

:heavy_check_mark: 201 Ok Created
:x: 403 Unauthorized
:x: 422 Tipo de sesión is mandatory

# Student Evaluation

## Actions for StudentEvaluation Controller

| Verb   | URI                     | Action  | Route Name                              | Response Type                  |
| ------ | ------------------------| ------- | --------------------------------------- | ------------------------------ |
| GET    | `/api/2/evaluation`     | list    | `StudentEvaluationController#list()`    | [`ListWCursor<T>`](#responses) |
| GET    | `/api/2/evaluation/{id}`| single  | `StudentEvaluationController#single()`  | `<T>`                          |

Here, `<T>` equals to the `Type`, and in this particular case is `StudentEvaluationDomain` (defined [below](#type-definition-studentevaluation))

### Type definition StudentEvaluation

```ts


interface StudentEvaluationDomain {
  id: long;
  idAlumno: string;
  idSesionProgramada: string;
  calificacion: float;
  obPatternsEvaluados: string[];
  obPatternsEvaluadosObjs?: ObPatternEvaluadoDomain[]; 
  createdOn?: Date,
	updatedOn?: Date,
}



```

Example payload:

```json
{
  "id": "67801",
  "idAlumno": "3423",
  "idSesionProgramada": "13123456",
  "calificacion": 8.5,
  "obPatternsEvaluados": ["pattern001", "pattern002"],
  "obPatternsEvaluadosObjs": [
    {
      "id": "132001",
  "idObPattern": "566123",
  "type": "scale",
  "calificacion": 8.5,
  "obPatternName": "Nombre especifico",
  "obPatternDescription": "Descripcion detallada"

    },
    {
      "id": "12301",
  "idObPattern": "88123",
  "type": "scale",
  "calificacion": 9.5,
  "obPatternName": "Otro nombre",
  "obPatternDescription": "Ejemplo de una Descripcion"

    }
  ]
}




```

:heavy_check_mark: 201 Ok Created
:x: 403 Unauthorized
:x: 422 ObPattern is required by new ObPatternEval

# Task Time

## Actions for TaskTime Controller

| Verb   | URI                     | Action  | Route Name                     | Response Type                  |
| ------ | ----------------------  | ------- | -------------------------------| ------------------------------ |
| GET    | `/api/2/TasksTime`      | list    | `TaskTimeController#list()`    | [`ListWCursor<T>`](#responses) |
| GET    | `/api/2/TasksTime/{id}` | single  | `TaskTimeController#single()`  | `<T>`                          |

Here, `<T>` equals to the `Type`, and in this particular case is `TaskTimeDomain` (defined [below](#type-definition-taskstime))

### Type definition TasksTime

```ts
interface TaskTime {
  id: long;
  idTask: string;
  idSession: string;
  minutes: int;
  createdOn?: Date,
	updatedOn?: Date,
}



```

Example payload:

```json
{
  "id": "12301",
  "idTask": "123123",
  "idSession": "1236",
  "minutes": 90
}





```

:heavy_check_mark: 201 Ok Created
:x: 403 Unauthorized

# Task Type

## Actions for TaskType Controller

| Verb   | URI                      | Action  | Route Name                     | Response Type                  |
| ------ | -------------------------| ------- | -------------------------------| ------------------------------ |
| GET    | `/api/2/task-types`      | list    | `TaskTypeController#list()`    | [`ListWCursor<T>`](#responses) |
| GET    | `/api/2/task-types/{id}` | single  | `TaskTypeController#single()`  | `<T>`                          |

Here, `<T>` equals to the `Type`, and in this particular case is `TaskType` (defined [below](#type-definition-tasktype))

### Type definition TaskType

```ts
interface TaskType {
  id: long;
  description: string;
  createdOn?: Date,
	updatedOn?: Date,
}



```

Example payload:

```json
{
  "id": "432401",
  "description": "Descripción del tipo de tarea"
}



```

:heavy_check_mark: 201 Ok Created
:x: 403 Unauthorized
:x: 422 Task type description is mandatory

# Team Evaluation

## Actions for TeamEvaluation Controller

| Verb   | URI                           | Action  | Route Name                           | Response Type                  |
| ------ | ------------------------------| ------- | -----------------------------------  | ------------------------------ |
| GET    | `/api/2/team-evaluation`      | list    | `TeamEvaluationController#list()`    | [`ListWCursor<T>`](#responses) |
| GET    | `/api/2/team-evaluation/{id}` | single  | `TeamEvaluationController#single()`  | `<T>`                          |

Here, `<T>` equals to the `Type`, and in this particular case is `TeamEvaluation` (defined [below](#type-definition-teamevaluation))

### Type definition TeamEvaluation

```ts
interface TeamEvaluationDomain {
  id: long;
  idTeam: string;
  idSesionProgramada: string;
  calificacion: float;
  StudentsEvaluados: string[];
  StudentsEvaluadosObjs: IStudentEvaluationDomain[];
  createdOn?: Date,
	updatedOn?: Date,
}




```

Example payload:

```json
{
  "id": "teamEval001",
  "idTeam": "team123",
  "idSesionProgramada": "session456",
  "calificacion": 8.5,
  "StudentsEvaluados": [
    "student1",
    "student2"
  ],
  "StudentsEvaluadosObjs": [
    {
      "id": "98701",
      "idSesionProgramada": "8796",
      "calificacion": 8.0

      ]

    }
```

:heavy_check_mark: 201 Ok Created
:x: 403 Unauthorized
:x: 422 Sesion is required by new Team Evaluation Completed
:x: 422 Team is required by new Team Evaluation Completed

# Team Evaluation V2

## Actions for TeamEvaluationV2 Controller

| Verb   | URI                              | Action  | Route Name                             | Response Type                  |
| ------ | -------------------------------- | ------- | ---------------------------------------| ------------------------------ |
| GET    | `/api/2/team-evaluation-v2`      | list    | `TeamEvaluationV2Controller#list()`    | [`ListWCursor<T>`](#responses) |
| GET    | `/api/2/team-evaluation-v2/{id}` | single  | `TeamEvaluationV2Controller#single()`  | `<T>`                          |

Here, `<T>` equals to the `Type`, and in this particular case is `TeamEvaluationV2` (defined [below](#type-definition-teamevaluationv2))

### Type definition TeamEvaluationV2

```ts
interface TeamEvaluationV2 {
  id: long;
  teamId: string;
  sesionId: string;
  scheduledSessionId: string;
  teamAverage: double;
  evaluationByPlayer?: Dictionary<GradeByPattern>;
  averageByPlayer?: Dictionary<double>;
  commentByPlayer?: Dictionary<string>;
  status: string;
  evaluations: PlayerEvaluation[];
  involvedPlayerIds: string[];
  createdOn?: Date,
	updatedOn?: Date,
}





```

Example payload:

```json
{
  "id": "2001",
  "teamId": "43123",
  "sesionId": "456",
  "scheduledSessionId": "3489",
  "teamAverage": 8.4,
  "evaluationByPlayer": {
    "player1": {
      "pattern1": 7.5,
      "pattern2": 8.0
    },
    "player2": {
      "pattern3": 9.0
    }
  },
  "averageByPlayer": {
    "player1": 7.75,
    "player2": 9.0
  },
  "commentByPlayer": {
    "player1": "Debes mejorar.",
    "player2": "Excelente Sigue asiii."
  },
  "status": "open",  //open/edited/closed
  "evaluations": [
    {
      "id": "2132301",
      "evaluationDetails": [
        {
        
          "average": 7.5
        },
        {
      
          "average": 8.0
        }
      ]
    },
    {
      "id": "6546002",
      "evaluationDetails": [
        {
          "Id": "345503",
          "average": 9.0
        }
      ]
    }
  ],
  "involvedPlayerIds": [
    "12301",
    "32102"
  ]
}

```

:heavy_check_mark: 201 Ok Created
:x: 403 Unauthorized
:x: 422 'scheduledSessionId' is required by new TeamEvaluationV2
:x: 422 'teamId' is required by new TeamEvaluationV2

# Tema

## Actions for Tema Controller

| Verb   | URI                 | Action  | Route Name                 | Response Type                  |
| ------ | ------------------- | ------- | ---------------------------| ------------------------------ |
| GET    | `/api/2/tema`       | list    | `TemaController#list()`    | [`ListWCursor<T>`](#responses) |
| GET    | `/api/2/tema/{id}`  | single  | `TemaController#single()`  | `<T>`                          |

Here, `<T>` equals to the `Type`, and in this particular case is `TemaDomain` (defined [below](#type-definition-tema))

### Type definition Tema

```ts
interface Tema {
  id: long;
  titulo: string;
  descripcion: string;
  createdOn?: Date,
	updatedOn?: Date,
}






```

Example payload:

```json
{
  "id": "3431",
  "titulo": "Introducción al Ataque",
  "descripcion": "Este tema cubre los fundamentos básicos asi como algunos trucos de como atacar correctamente."
}


```

:heavy_check_mark: 201 Ok Created
:x: 403 Unauthorized

# Temporada Bitacora

## Actions for TemporadaBitacora Controller

| Verb   | URI                              | Action  | Route Name                              | Response Type                  |
| ------ | ---------------------------------| --------| ----------------------------------------| ------------------------------ |
| GET    | `/api/2/temporada-bitacora`      | list    | `TemporadaBitacoraController#list()`    | [`ListWCursor<T>`](#responses) |
| GET    | `/api/2/temporada-bitacora/{id}` | single  | `TemporadaBitacoraController#single()`  | `<T>`                          |

Here, `<T>` equals to the `Type`, and in this particular case is `TemporadaBitacoraDomain` (defined [below](#type-definition-temporadabitacora))

### Type definition TemporadaBitacora

```ts
interface TemporadaBitacora {
  id: long;
  teamid: string;
  temporadaid: string;
  estado: boolean;
  createdOn?: Date,
	updatedOn?: Date,
}

```

Example payload:

```json
{
  "id": "321301",
  "teamid": "31223",
  "temporadaid": "2023",
  "estado": true
}


```

:heavy_check_mark: 201 Ok Created
:x: 403 Unauthorized

# Temporada

## Actions for Temporada Controller

| Verb   | URI                                | Action            | Route Name                                               | Response Type                  |
| ------ | ---------------------------------- | ------------------|----------------------------------------------------------| ------------------------------ |
| GET    | `/api/2/temporada`                 | list              | `TemporadaController#list()`                             | [`ListWCursor<T>`](#responses) |
| GET    | `/api/2/temporada/{id}`            | single            | `TemporadaController#single()`                           | `<T>`                          |
| GET    | `/api/2/temporada/current-season`  | currentSeason     | `TemporadaController#currentSeason()`                    | `<T>`                          |

Here, `<T>` equals to the `Type`, and in this particular case is `TemporadaDomain` (defined [below](#type-definition-temporada))

### Type definition Temporada

```ts
interface Temporada {
  id: long;
  nombre: string;
  estado: boolean;
  fechaIni: string;  
  fechaFin: string;  
  cerrado: string;   
  createdOn?: Date,
	updatedOn?: Date,
}

```

Example payload:

```json
{
  "id": "32001",
  "nombre": "Temporada 2024",
  "estado": true,
  "fechaIni": "2024-01-01",
  "fechaFin": "2024-12-31",
  "cerrado": "2024-12-31"
}



```

:heavy_check_mark: 201 Ok Created
:x: 403 Unauthorized

# Test Entity

## Actions for TestEntity Controller

| Verb   | URI                                | Action       | Route Name                            | Response Type                  |
| ------ | -----------------------------------| -------------| --------------------------------------| ------------------------------ |
| GET    | `/api/2/test-entities`             | list         | `TestEntityController#list()`         | [`ListWCursor<T>`](#responses) |
| GET    | `/api/2/test-entities/{id}`        | single       | `TestEntityController#single()`       | `<T>`                          |
| GET    | `/api/2/test-entities/check-models`| checkModels  | `TestEntityController#checkModels()`  | `<T>`                          |

Here, `<T>` equals to the `Type`, and in this particular case is `TestEntityDomain` (defined [below](#type-definition-testentity))

### Type definition TestEntity

```ts
interface TestEntity {
  id: long;
  field: string;
  intIds: string[]; 
  strIds: string[];
  strList: string[];
  longList: int[]; 
  createdOn?: Date,
	updatedOn?: Date,
}

```

Example payload:

```json
{
  "id": "12301",
  "field": "Ejemplo de Campo",
  "intIds": ["134", "1232","3"," 1234"],
  "strIds": ["1123", "32321", "23123"],
  "strList": ["item1", "item2", "item3"],
  "longList": [10000000001, 10000000002],
}



```

:heavy_check_mark: 201 Ok Created
:x: 403 Unauthorized

# Training Completed

## Actions for TrainingCompleted Controller

| Verb   | URI                                       | Action     | Route Name                                                                    | Response Type                  |
| ------ | ------------------------------------------| ---------- | ------------------------------------------------------------------------------| ------------------------------ |
| GET    | `/api/2/training-completed`               | list       | `TrainingCompletedController#list()`                                          | [`ListWCursor<T>`](#responses) |
| GET    | `/api/2/training-completed/{id}`          | single     | `TrainingCompletedController#single()`                                        | `<T>`                          |
| GET    | `/api/2/training-completed/export`        | export| `TrainingCompletedController#export()`                                             | [`List<OrgDomain>`]            |
| GET    | `/api/2/training-completed/export-table`  | exportTable| `TrainingCompletedController#exportTable()`                                   | `<T>`                          |

Here, `<T>` equals to the `Type`, and in this particular case is `TrainingCompletedDomain` (defined [below](#type-definition-trainingcompleted))

### Type definition TrainingCompleted

```ts
interface TrainingCompleted {
  id: long;
  idSesionProgramada: string;
  list: int[]; 
  playerAttendance: string[]; 
  task: int[]; 
  playersAggregated: string[];
  comment: string;
  commentByPlayer: { [key: string]: string }; 
  tasksPerformed: { [key: string]: int[] }; 
  reasonByPlayer: { [key: string]: int }; 
  commentByTask: { [key: string]: string }; 
  createdOn?: Date,
	updatedOn?: Date,
}


```

Example payload:

```json
{
  "id": "56001",
  "idSesionProgramada": "6543",
  "list": [1001, 1002, 1003],
  "playerAttendance": ["2001", "2002", "2003"],
  "task": [3001, 3002],
  "playersAggregated": ["player1", "player2"],
  "comment": "Entrenamiento Completado.",
  "commentByPlayer": {
    "player1": "Bun trabajo.",
    "player2": "Necesitas mejorar."
  },
  "tasksPerformed": {
    "task1": [4001, 4002],
    "task2": [4003]
  },
  "reasonByPlayer": {
    "player1": 5001,
    "player2": 5002
  },
  "commentByTask": {
    "task1": "Tarea completada a tiempo.",
    "task2": "La tarea estuvo complicada."
  }
}




```

:heavy_check_mark: 201 Ok Created
:x: 403 Unauthorized
:x: 422 Session is required by new Training Completed

--------------------------------------------------

# Request and Response Types

## Requests

## Responses

```ts
interface ListWCursor<T> {
  entities: T[];
  cursor: string;
}
```

And an example response of `ListWCursor<Criterion>` would be:

```json
{
  "entities": [
    {
      "id": 1,
      "tag": "velocidad",
      "type": "scale",
      "scale": 100,
      "createdOn": "2020-02-12T12:30:15Z",
      "updatedOn": "2020-02-12T12:30:15Z"
    },
    {
      "id": 2,
      "tag": "pase",
      "type": "likert",
      "scale": null,
      "createdOn": "2020-02-12T12:31:20Z",
      "updatedOn": "2020-02-12T12:31:20Z"
    },
    {
      "id": 3,
      "tag": "control",
      "type": "likert",
      "scale": null,
      "createdOn": "2020-02-12T12:32:30Z",
      "updatedOn": "2020-02-12T12:32:30Z"
    }
  ],
  "cursor": "CjMSLWoSc35jYXJuZXNjYXlhbC0yMDE4chcLEgpEb2N1bWVudG9zGICAgPTvoMQLDBgAIAA"
}
```
