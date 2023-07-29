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
const getAllStudentd = (req, res) => {
  let response = {
    status: "success",
    data: { students },
  };
  res.status(200).json(response);
};
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
const createStudent = (req, res) => {
  let body = req.body;
  let id = students[students.length - 1].id + 1;
  let obj = Object.assign({ id: id }, body);
  students.push(obj);
  fs.writeFileSync("students.json", JSON.stringify(students), "utf-8");
  res.send("created");
};
const createTeacher = (req, res) => {
  let body = req.body;
  let id = teachers[teachers.length - 1].id + 1;
  let obj = Object.assign({ id: id }, body);
  teachers.push(obj);
  fs.writeFileSync("teachers.json", JSON.stringify(students), "utf-8");
  res.send("created");
};
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

const updateStudentByID = (req, res) => {
  let id = req.params.id * 1;
  let index = students.findIndex((student) => {
    return student.id === id;
  });
  students[index] = Object.assign({}, req.body);
  fs.writeFileSync("students.json", JSON.stringify(students), "utf-8");
  res.send("sent");
};
const updateTeacherByID = (req, res) => {
  let id = req.params.id * 1;
  let index = teachers.findIndex((teacher) => {
    return teacher.id === id;
  });
  teachers[index] = Object.assign({}, req.body);
  fs.writeFileSync("teachers.json", JSON.stringify(teachers), "utf-8");
  res.send("sent");
};

const deleteStudentByID = (req, res) => {
  let id = req.params.id * 1;
  let index = students.findIndex((student) => {
    return student.id === id;
  });

  students.splice(index, 1);
  fs.writeFileSync("students.json", JSON.stringify(students), "utf-8");
  res.send("deleted");
};
const deleteTeacherByID = (req, res) => {
  let id = req.params.id * 1;
  let index = teachers.findIndex((teacher) => {
    return teacher.id === id;
  });

  teachers.splice(index, 1);
  fs.writeFileSync("teachers.json", JSON.stringify(teachers), "utf-8");
  res.send("deleted");
};

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

const deleteBulkStudentsByID = (req, res) => {
  const ids = Number(req.params.id);
  // ids.split(",")
  console.log(ids[1]);
  // console.log(ids[1]);
  for (let id of ids) {
    // console.log(id);
    let index = students.findIndex((student) => {
      return student.id === id;
    });
    students.splice(index, 1);
  }
  fs.writeFileSync("students.json", JSON.stringify(students), "utf-8");
};

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
