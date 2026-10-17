---
title: 'Node.js - 8. 使用数据库'
description: 'Learn Node.js'
pubDate: '2026-10-17'
---

#### 使用 Docker
```bash
docker run \
  --name mysql \
  -v db-data:/etc/mysql/conf.d \
  -e MYSQL_ROOT_PASSWORD=topSecret \
  -e MYSQL_ROOT_HOST=% -p 3306:3306 \
  -d \
  mysql:latest
```
- docker run 命令根据名为 mysql:latest 的镜像创建一个新容器。MySQL 官方在 Docker Hub（https://hub.docker.com/）上提供了 mysql 镜像。执行该命令时，会下载指定版本的镜像，此处的 :latest 表示最新版本。
- --name 选项为容器指定一个名称，便于管理，这样就不必通过容器 ID 来引用它。-v 选项用于指定将一个卷挂载到容器中，使数据库文件存储在宿主机上，而不是容器内部。
- 接下来的 -e 选项用于设置容器的环境变量。在本例中，MySQL 使用这些变量设置 root 用户的密码，并允许 root 用户从容器外部登录。
- 最后，使用 -d 选项指定在后台运行容器。执行命令后，你会得到一串看起来难以理解的字符，这就是容器的 ID。除了容器名称，你也可以使用该 ID 管理容器。
| 命令                         | 说明         |
| -------------------------- | ---------- |
| `docker run <image>`       | 创建并运行一个新容器 |
| `docker rm <container>`    | 删除指定容器     |
| `docker rmi <image>`       | 删除指定镜像     |
| `docker ps`                | 显示所有容器的列表  |
| `docker stop <container>`  | 停止容器       |
| `docker start <container>` | 启动已停止的容器   |
所有命令的完整列表，包括命令说明和可用选项，请参阅官方文档：[https://docs.docker.com/engine/reference/commandline/cli/](https://docs.docker.com/engine/reference/commandline/cli/)。

#### SQLite
##### 安装并创建数据库
```bash
npm install sqlite3

$ sqlite3 movie.db
Enter ".help" for usage hints.
sqlite> CREATE TABLE `Movies` (
   ...> id INTEGER PRIMARY KEY,
   ...> title TEXT,
   ...> year INTEGER);
sqlite> INSERT INTO `Movies` (`title`, `year`) VALUES
   ...> ('Iron Man', 2008),
   ...> ('Thor', 2011),
   ...> ('Captain America', 2011);
sqlite>
```
##### 建立连接和使用
```js
import sqlite from 'sqlite3';

const db = new sqlite.Database('./movie.db');

export async function getAll() {
  return new Promise((resolve, reject) => {
    const query = 'SELECT * FROM Movies';
    db.all(query, (error, results) => {
      if (error) {
        reject(error);
      } else {
        resolve(results);
      }
    });
  });
}
export async function get(id) {
  return new Promise((resolve, reject) => {
    const query = 'SELECT * FROM Movies WHERE id = ?';
    db.get(query, [id], (error, results) => {
      if (error) {
        reject(error);
      } else {
        resolve(results);
      }
    });
  });
}
export async function save(movie) {
  if (!movie.id) {
    return insert(movie);
  } else {
    return update(movie);
  }
}

export async function remove(id) {}
function insert(movie) {
  return new Promise((resolve, reject) => {
    const query = 'INSERT INTO Movies (title, year) VALUES (?, ?)';
    db.run(query, [movie.title, movie.year], function (error) {
      if (error) {
        reject(error);
      } else {
        resolve({ ...movie, id: this.lastID });
      }
    });
  });
}
export async function remove(id) {
  return new Promise((resolve, reject) => {
    const query = 'DELETE FROM Movies WHERE id = ?';
    db.run(query, [id], (error, results) => {
      if (error) {
        reject(error);
      } else {
        resolve(results);
      }
    });
  });
}
function update(movie) {
  return new Promise((resolve, reject) => {
    const query = 'UPDATE Movies SET title = ?, year = ? WHERE id = ?';
    db.run(query, [movie.title, movie.year, movie.id], (error, results) => {
      if (error) {
        reject(error);
      } else {
        resolve(results);
      }
    });
  });
}
```
#### 对象关系映射
ORM 指的是将数据库中的关系映射为对象。
##### 安装和配置
```bash
npm install sequelize sqlite3
```
```js
import { Sequelize } from 'sequelize';

const sequelize = new Sequelize({
  dialect: 'sqlite',
  storage: './movie.db',
});

const Movies = sequelize.define(
  'Movies',
  {
    title: {
      type: Sequelize.STRING,
    },
    year: {
      type: Sequelize.INTEGER,
    },
  },
  { timestamps: false },
);

export function getAll() {
  return Movies.findAll();
}

export function get(id) {
  return Movies.findByPk(id);
}

export function remove(id) {
  return Movies.destroy({ where: { id } });
}

export function save(movie) {
  return Movies.upsert(movie);
}
```
#### MongoDB
MongoDB 是一种典型的文档型数据库。
##### 启动 MongoDB 容器、安装
```bash
docker run \
  --name mongo \
  -v db-data:/data/db \
  -p 27017:27017 \
  -d \
  mongo:latest
```
```bash
npm install mongodb
```
##### 使用
```js
import { MongoClient } from 'mongodb';

let collection = null;

async function connect() {
  if (collection) {
    return collection;
  }
  const client = new MongoClient('mongodb://localhost:27017');

  await client.connect();

  const db = client.db('moviedb');
  collection = db.collection('Movie');

  return collection;
}

export async function getAll() {
  const collection = await connect();
  const docs = await collection.find({});
  return docs.toArray();
}
export async function get(id) {
  const collection = await connect();
  const doc = await collection.findOne({ id });
  return doc;
}
export async function remove(id) {
  const collection = await connect();
  return collection.deleteOne({ id });
}
export function save(movie) {
  if (!movie.id) {
    return insert(movie);
  } else {
    return update(movie);
  }
}

async function insert(movie) {
  movie.id = Date.now();
  const collection = await connect();
  const data = collection.insertOne(movie);
  return data;
}

async function update(movie) {
  movie.id = parseInt(movie.id, 10);
  const collection = await connect();
  await collection.updateOne({ id: movie.id }, { $set: movie });
  return movie;
}
```
#### Mongoose
Mongoose 允许你在应用程序中直接将数据库中的文档视为对象，并提供了多种处理数据的方法。
```bash
npm install mongoose
```
```js
import mongoose from 'mongoose';

mongoose.connect('mongodb://localhost:27017/moviedb', {
  useNewUrlParser: true,
  useUnifiedTopology: true,
});

const Movie = mongoose.model('Movie', {
  id: Number,
  title: String,
  year: Number,
});

export function getAll() {
  return Movie.find({});
}

export function get(id) {
  return Movie.findOne({ id });
}

export async function remove(id) {
  const movie = await get(id);
  return movie.remove();
}

export async function save(movie) {
  if (!movie.id) {
    const newMovie = new Movie(movie);
    newMovie.id = Date.now();
    return newMovie.save();
  } else {
    const existingMovie = await get(parseInt(movie.id, 10));
    existingMovie.title = movie.title;
    existingMovie.year = movie.year;
    return existingMovie.save();
  }
}
```