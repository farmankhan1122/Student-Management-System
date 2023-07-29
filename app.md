# Student Management System

## importing mudules
```javascript
const express = require("express");
const fs = require("fs");
const path = "/api/v1";
const app = express();
app.use(express.json());
let students = JSON.parse(fs.readFileSync("students.json", "utf-8"));
let teachers = JSON.parse(fs.readFileSync("teachers.json", "utf-8"));
let batches = JSON.parse(fs.readFileSync("batches.json", "utf-8"));
app.listen(4000, "127.0.0.1", () => {
  console.log("server started....");
});
```
## Get All Students
```javascript
const getAllStudentd = (req, res) => {
  let response = {
    status: "success",
    data: { students },
  };
  res.status(200).json(response);
};
```
## Get All Teachers
```javascript
const getAllTeachers = (req, res) => {
  let response = {
    status: "success",
    data: { teachers },
  };
  res.status(200).json(response);
};
const getAllBatches = (req, res) => {
  let response = {
    status: "success",
    data: { batches },
  };
  res.status(200).json(response);
};
```
## Create Student
```javascript
const createStudent = (req, res) => {
  let body = req.body;
  let id = students[students.length - 1].id + 1;
  let obj = Object.assign({ id: id }, body);
  students.push(obj);
  fs.writeFileSync("students.json", JSON.stringify(students), "utf-8");
  res.send("created");
};
```


## Create Teacher
```javascript
const createTeacher = (req, res) => {
  let body = req.body;
  let id = teachers[teachers.length - 1].id + 1;
  let obj = Object.assign({ id: id }, body);
  teachers.push(obj);
  fs.writeFileSync("teachers.json", JSON.stringify(students), "utf-8");
  res.send("created");
};
```
## Create Bulk Students
```javascript
const createBulkStudent = (req, res) => {
  let body = req.body;
  for (let student of body) {
    let id = students[students.length - 1].id + 1;
    let obj = Object.assign({ id: id }, student);
    students.push(obj);
  }
  fs.writeFileSync("students.json", JSON.stringify(students), "utf-8");
  res.send("created");
};
```
## Create Bulk Teachers
```javascript
const createBulkTeacher = (req, res) => {
  let body = req.body;
  for (let teacher of body) {
    let id = teachers[teachers.length - 1].id + 1;
    let obj = Object.assign({ id: id }, teacher);
    teachers.push(obj);
  }
  fs.writeFileSync("teachers.json", JSON.stringify(teachers), "utf-8");
  res.send("created");
};
```
## Get Student By ID
```javascript
const getStudentByID = (req, res) => {
  let id = req.params.id * 1;
  let student = students.filter((student) => {
    return student.id === id;
  });
  if (!student || student.length === 0) {
    res.status(404).send({
      status: "error",
      message: "student not found",
      data: {},
    });
  }
  res.status(200).send({
    status: "success",
    data: { student: student[0] },
  });
};
```
## Get Teacher By ID
```javascript
const getTeacherByID = (req, res) => {
  let id = req.params.id * 1;
  let teacher = teachers.filter((teacher) => {
    return teacher.id === id;
  });
  if (!teacher || teacher.length === 0) {
    res.status(404).send({
      status: "error",
      message: "teacher not found",
      data: {},
    });
  }
  res.status(200).send({
    status: "success",
    data: { teacher: teacher[0] },
  });
};
```
## Update Student By ID
```javascript
const updateStudentByID = (req, res) => {
  let id = req.params.id * 1;
  let index = students.findIndex((student) => {
    return student.id === id;
  });
  students[index] = Object.assign({}, req.body);
  fs.writeFileSync("students.json", JSON.stringify(students), "utf-8");
  res.send("sent");
};
```
## Update Teacher By ID
```javascript
const updateTeacherByID = (req, res) => {
  let id = req.params.id * 1;
  let index = teachers.findIndex((teacher) => {
    return teacher.id === id;
  });
  teachers[index] = Object.assign({}, req.body);
  fs.writeFileSync("teachers.json", JSON.stringify(teachers), "utf-8");
  res.send("sent");
};
```
## Delete Student By ID
```javascript
const deleteStudentByID = (req, res) => {
  let id = req.params.id * 1;
  let index = students.findIndex((student) => {
    return student.id === id;
  });

  students.splice(index, 1);
  fs.writeFileSync("students.json", JSON.stringify(students), "utf-8");
  res.send("deleted");
};
```
## Delete Teacher By ID
```javascript
const deleteTeacherByID = (req, res) => {
  let id = req.params.id * 1;
  let index = teachers.findIndex((teacher) => {
    return teacher.id === id;
  });

  teachers.splice(index, 1);
  fs.writeFileSync("teachers.json", JSON.stringify(teachers), "utf-8");
  res.send("deleted");
};
```
## Update Bulk Students By ID's
```javascript
const updateBulkStudentsByID = (req, res) => {
  let body = req.body;
  for (let value of body) {
    let id = value.id;
    let index = students.findIndex((student) => {
      return student.id === id;
    });
    students[index] = value;
    fs.writeFileSync("students.json", JSON.stringify(students), "utf-8");
  }
};
```
## Update Bulk Teachers By ID's
```javascript
const updateBulkTeachersByID = (req, res) => {
  let body = req.body;
  for (let value of body) {
    let id = value.id;
    let index = teachers.findIndex((teacher) => {
      return teacher.id === id;
    });
    teachers[index] = value;
    fs.writeFileSync("teachers.json", JSON.stringify(teachers), "utf-8");
  }
};
```

## URLs
```javascript

app.get(`${path}/students`, getAllStudentd);
app.get(`${path}/teachers`, getAllTeachers);
app.get(`${path}/batches`, getAllBatches);
app.post(`${path}/student`, createStudent);
app.post(`${path}teacher`, createTeacher);
app.post(`${path}students`, createBulkStudent);
app.post(`${path}teachers`, createBulkTeacher);
app.get(`${path}/students/:id`, getStudentByID);
app.get(`${path}/teachers/:id`, getStudentByID);
app.put(`${path}/students/:id`, updateStudentByID);
app.put(`${path}/teachers/:id`, updateTeacherByID);
app.delete(`${path}/teachers/:id`, deleteTeacherByID);
app.put(`${path}/studentss/bulk`, updateBulkStudentsByID);
app.put(`${path}/teacherss/bulk`, updateBulkTeachersByID);
app.delete(`${path}/students/:id`, deleteBulkStudentsByID);
```
