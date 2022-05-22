---
title: "database"
draft: true
---
### sqlalchemy

```sh
# pip install psycopg2-binary
# sqlacodegen postgres://user:passwd@host:ip/database --outfile model.py
```

```sh
from sqlalchemy import create_engine
from sqlalchemy import Column, String
from sqlalchemy.ext.declarative import declarative_base
from sqlalchemy.orm import sessionmaker

db_string = "postgres://admin:donotusethispassword@aws-us-east-1-portal.19.dblayer.com:15813/compose"

db = create_engine(db_string)
base = declarative_base()


class Film(base):
    __tablename__ = "films"

    title = Column(String, primary_key=True)
    director = Column(String)
    year = Column(String)


Session = sessionmaker(db)
session = Session()

base.metadata.create_all(db)

# Create
doctor_strange = Film(title="Doctor Strange", director="Scott Derrickson", year="2016")
session.add(doctor_strange)
session.commit()

# Read
films = session.query(Film)
for film in films:
    print(film.title)

# Update
doctor_strange.title = "Some2016Film"
session.commit()

# Delete
session.delete(doctor_strange)
session.commit()

delete_obj = Shop.__table__.delete().where(Shop.shop_cate.contains("m"))
session.execute(delete_obj)
session.commit()
```

```sh
#  from .base_model import Base
from sqlalchemy.orm import sessionmaker, scoped_session
from contextlib import contextmanager
from sqlalchemy.ext.declarative import declarative_base
from sqlalchemy import Column, Integer, String, TIMESTAMP, text, JSON

from sqlalchemy import create_engine


Base = declarative_base()


class PyOrmModel(Base):
    __tablename__ = "py_orm"

    id = Column(Integer, autoincrement=True, primary_key=True, comment="唯一id")
    name = Column(String(255), nullable=False, default="", comment="名称")
    attr = Column(JSON, nullable=False, comment="属性")
    ct = Column(TIMESTAMP, nullable=False, server_default=text("CURRENT_TIMESTAMP"), comment="创建时间")
    ut = Column(TIMESTAMP, nullable=False, server_default=text("CURRENT_TIMESTAMP"), comment="更新时间")

    @staticmethod
    def fields():
        return ["id", "name", "attr"]

    @staticmethod
    def to_json(model):
        fields = PyOrmModel.fields()
        json_data = {}
        for field in fields:
            json_data[field] = model.__getattribute__(field)
        return json_data

    @staticmethod
    def from_json(data: dict):
        fields = PyOrmModel.fields()

        model = PyOrmModel()
        for field in fields:
            if field in data:
                model.__setattr__(field, data[field])
        return model

db_url = "postgresql://user:password@ip:5432/db"
engine = create_engine(db_url, echo=False)
Base.metadata.create_all(engine)


def _get_session():
    """获取session"""
    return scoped_session(sessionmaker(bind=engine, expire_on_commit=False))()


# 在这里对session进行统一管理，包括获取，提交，回滚和关闭
@contextmanager
def db_session(commit=True):
    session = _get_session()
    try:
        yield session
        if commit:
            session.commit()
    except Exception as e:
        session.rollback()
        raise e
    finally:
        if session:
            session.close()


class PyOrmModelOp:
    def __init__(self):
        pass

    @staticmethod
    def save_data(data: dict):
        with db_session() as session:
            model = PyOrmModel.from_json(data)
            session.add(model)

    # 查询操作，不需要commit
    @staticmethod
    def query_data(pid: int):
        data_list = []
        with db_session(commit=False) as session:
            data = session.query(PyOrmModel).filter(PyOrmModel.id == pid)
            for d in data:
                data_list.append(PyOrmModel.to_json(d))

            return data_list


for i in range(9):
    #  PyOrmModelOp.save_data({"id": i, "name": "test", "attr": {}})
    PyOrmModelOp.save_data({"name": "test", "attr": {}})
    result = PyOrmModelOp.query_data(i)
    print(result)
```

```sh
# pip install 'databases[aiomysql]' aiomysq
import asyncio

# Create a database instance, and connect to it.
from databases import Database


async def run():
    db_url = "mysql://user:passwd@host:port/db"
    database = Database(db_url)
    #  database = Database("sqlite+aiosqlite:///example.db")
    await database.connect()

    # Create a table.
    #  query = """CREATE TABLE HighScores (id INTEGER PRIMARY KEY AUTO_INCREMENT, name VARCHAR(100), score INTEGER)"""
    #  await database.execute(query=query)

    # Insert some data.
    query = "INSERT INTO HighScores(name, score) VALUES (:name, :score)"
    values = [
        {"name": "Daisy", "score": 92},
        {"name": "Neil", "score": 87},
        {"name": "Carol", "score": 43},
    ]
    await database.execute_many(query=query, values=values)

    # Run a database query.
    query = "SELECT * FROM HighScores"
    rows = await database.fetch_all(query=query)
    print("High Scores:", rows)
    for r in rows:
        print(r)
    return rows


loop = asyncio.get_event_loop()
loop.run_until_complete(run())
```
