# lab05
# Копирование репозитория из предыдущей работы
```sh
dmitrii@DESKTOP-9P3LE74:~/Dmitriiagishev/workspace/projects/lab051$ git clone https://github.com/Dmitriiagishev/lab03.git
Cloning into 'lab03'...
remote: Enumerating objects: 291, done.
remote: Counting objects: 100% (62/62), done.
remote: Compressing objects: 100% (48/48), done.
remote: Total 291 (delta 21), reused 32 (delta 9), pack-reused 229
Receiving objects: 100% (291/291), 114.66 KiB | 221.00 KiB/s, done.
Resolving deltas: 100% (145/145), done.
```
# Создание каталога .github/workflows
```sh
dmitrii@DESKTOP-9P3LE74:~/Dmitriiagishev/workspace/projects/lab051$ mkdir .github
dmitrii@DESKTOP-9P3LE74:~/Dmitriiagishev/workspace/projects/lab051$ cd .github
dmitrii@DESKTOP-9P3LE74:~/Dmitriiagishev/workspace/projects/lab051/.github$ mkdir workflows
dmitrii@DESKTOP-9P3LE74:~/Dmitriiagishev/workspace/projects/lab051/.github$ cd workflows
```
# Создание файла Cl.yml
```sh
dmitrii@DESKTOP-9P3LE74:~/Dmitriiagishev/workspace/projects/lab051/.github/workflows$ cat > CI.yml
name: CMake

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

jobs:
  build_Linux:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v3

      - name: Configure Solver
        run: cmake ${{github.workspace}}/solver_application/ -B ${{github.workspace}}/solver_application/build

      - name: Build Solver
        run: cmake --build ${{github.workspace}}/solver_application/build

      - name: Configure HelloWorld
        run: cmake ${{github.workspace}}/hello_world_application/ -B ${{github.workspace}}/hello_world_application/build

      - name: Build HelloWorld
        run: cmake --build ${{github.workspace}}/hello_world_application/build

  build_Windows:
    runs-on: windows-latest

    steps:
      - uses: actions/checkout@v3

      - name: Configure Solver
        run: cmake ${{github.workspace}}/solver_application/ -B ${{github.workspace}}/solver_application/build

      - name: Build Solver
        run: cmake --build ${{github.workspace}}/solver_application/build

      - name: Configure HelloWorld
        run: cmake ${{github.workspace}}/hello_world_application/ -B ${{github.workspace}}/hello_world_application/build

      - name: Build HelloWorld
        run: cmake --build ${{github.workspace}}/hello_world_application/build

^Z
```
[![Coverage Status](https://coveralls.io/repos/github/ReZooty/lab05/badge.svg?branch=main)](https://coveralls.io/github/ReZooty/lab05?branch=main)
