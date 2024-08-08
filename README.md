1 Crear carpeta vacía
2 Seleccionar esa carpeta desde Visual Studio Code.
3 Añadir archivo index.js al directorio vacío
4 Ejecutar en consola el comando npm init (Inicia el proyecto como un NPM: colocar nombre del paquete, por ejemplo: nodejsapi, la versión dejar por defecto,
descripción: Opcional, el entry point (punto de entrada): Ya tiene por dfecto el index.js que creamos, comando de pruebas (test command): no tenemos,
git reository: agregar si se tiene, keywords: Omitir, author: Omitir, Is this OK? (yes) Enter (SE CREA EL PACKAGE JSON POR DEFECTO)).
5 Instalamos Express: npm install express (Se crea por defecto la carpeta node_modules y el archivo package-lock.json).
6 En el archivo index.js colocar:

const express = require("express");
const app = express();

app.use(express.json());

const students = [
  { id: 1, name: "Jorge", age: 20, enroll: true },
  { id: 2, name: "Mariana", age: 30, enroll: false },
  { id: 3, name: "Antonio", age: 25, enroll: false },
];

app.get("/", (req, res) => {
  res.send("Node JS api");
});

app.get("/api/students", (req, res) => {
  res.send(students);
});

app.get("/api/students/:id", (req, res) => {
  const student = students.find(
    (estudiante) => estudiante.id === parseInt(req.params.id)
  );
  if (!student) return res.status(404).send("Estudiante no encontrado.");
  else res.send(student);
});

app.post("/api/students", (req, res) => {
  const student = {
    id: students.length + 1,
    name: req.body.name,
    age: parseInt(req.body.age),
    enroll: req.body.enroll === "true",
  };

  students.push(student);
  res.send(student);
});

app.delete("/api/students/:id", (req, res) => {
  const student = students.find(
    (estudiante) => estudiante.id === parseInt(req.params.id)
  );
  if (!student) return req.status(404).send("Estudiante no encontrado.");

  const index = students.indexOf(student);
  students.splice(index, 1);
  res.send(student);
});

const port = process.env.port || 80;
app.listen(port, () => console.log(`Escuchando en el puerto ${port}...`));


7 Se puede ejecutar en postman ó algún explorador
8 Para ejecutar en el explorador escribimos en consola: node index.js
9 Una vez que esté escuchando vamos a Google Chrome y escribimos localhost + Enter (En este caso dice: Node JS api, porque optiene de app.get('/'))
10 Luego hacemos lo mismo pero escribimos en el navegador: localhost/api/students y obtenemos los datos del array students:

[
{
"id": 1,
"name": "Jorge",
"age": 20,
"enroll": true
},
{
"id": 2,
"name": "Mariana",
"age": 30,
"enroll": false
},
{
"id": 3,
"name": "Antonio",
"age": 25,
"enroll": false
}
]

11 Luego probamos con un id, colocamos en el navegador. Por ejemplo: localhost/api/students/1 (En este caso queremos ver el objeto que contiene el id: 1)
Y obtenemos:

{
"id": 1,
"name": "Jorge",
"age": 20,
"enroll": true
}

12 Si colocamos un id que no existe devuelve: Estudiante no encontrado.
13 Para ejecutarlo en Postman creamos una colección y probamos con GET, POST, DELETE.
14 Y listo. Luego queda subir la API a una base de datos que se encuentre en la nube, también hacer un docker con la API y tenerla en la nube.
