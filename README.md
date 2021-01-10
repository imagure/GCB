# GCB - TEST

This is a small application made for an evaluation.

From this repository you may download and run both Front and Back with docker-compose, using the command below.

```
docker-compose up --build
```

## GCB - Back | Commentaries

From what I could understand, this should be the main part of the evaluation. I wrote the code with typescript, and compiled using `tsc`. However, I didn't use typescript to its fullest, I don't have much experience working with it.

Besides that, I also opted to not use any ORM; I have some experience with them (mainly Objection.js), but instead I just made some persistence classes to use.

There's not much data validation being made as well, but I did some small things just to show: I'm using AJV to verify the requisition bodies at POST calls and some REGEX to validate uuid's.

I'm not using Express.js as well, I used Koa.js instead. I'm used to Koa.js and wanted to make it quicker. Their notation are similar tough and I shouldn't have a hard time rewriting it using Express.js.

To run the tests on a container, you may use:

```
cd gcb_back
docker-compose run -T app ./scripts/run_tests.sh
```

To run the Backend alone, you may use:

```
cd gcb_back
docker-compose up --build
```

The code goes as follows:

### /src/server:

Where you should find all the Koa.js related code.

### /src/controllers:

Where you should find the controllers for 'Doctors' and 'Specialties'.

### /src/facade:

Here you'll find classes with the required methods for 'Doctors' and 'Specialties'.

### /src/entities:

Here I made a persistence class to be extended and to create Doctors and Specialties as 'PersistedEntities'. If I had used an ORM, I would probably
have done this part differently.

### /src/persist:

Here you'll find all the "Knex" related code, with the "knex queries". You'll also find the Persistor, a class with a singleton pattern to register my reference for the database connection as a single instance.

### /dist/tests:

Here I have all the test code. It doesn't have full coverage, but it was enough to give me some certainty about the code behavior.

### Small API Documentation:

doctor_obj:
```
{
  id: uuid (only exists when Response body)
  name: String,
  tel: String,
  crm: String,
  cep: String,
  active: [active|inactive] (not necessary on request body)
  specialties: specialty_obj[] (only exists when Response body)
}
```

specialty_obj:
```
{
  id: uuid
  name: String,
  description: String
}
```

The application has the following endpoints:

### doctor endpoints:

| Endpoint  | Description  | Body  | Response  |
|:-:|---|---|---|
|GET/doctor/:doctor_id?  |  Read specific doctor from id or all doctors. | None  | if(success): {status: 200, body: {doctor: [{...doctor_obj},...] } } |
|GET/doctors/active   | Read Active Doctors  | None  | if(success): {status: 200, body: {doctor: [{...doctor_obj}]} } |
|POST/doctor   | Create or Update Doctor   | {doctor: {...doctor_obj} }  |  if(success): {status: 201, body: {id: id} } |
|DELETE/doctor/:doctor_id   | Soft Delete specific doctor  | None  |  if(success): {status: 204, body: None } |


### specialties endpoint:

| Endpoint  | Description  | Body  | Response  |
|:-:|---|---|---|
| GET/specialties  |  Read all specialties | None  | if(success): {status: 200, body: {specialties: [{...specialty_obj},...]}} } |


## GCB - Front | Commentaries

I wasn't sure if I needed to make a frontend for this evaluation and since I made this on the weekend I didn't want to bother anyone asking. So I did a small frontend using React.

It's not pretty and it doesn't has everything that was asked, like data validation on each input field, search for each field of information or the CORREIOS API request. Still, you should be able to use it and see the it all working together.
