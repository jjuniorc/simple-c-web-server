A Simple Webserver Written in C
===============================

This is a simple webserver written in the C programming language.

It uses a pool of 10 connections to serve multiple requests concurrently and it keeps track of how much data it has output, printing it to the standard output stream.


Compiling and Using the System
==============================

gcc server.c -o server

To run the server type ./server into a terminal that is in the directory where the executable file is located.

By default the server runs on port 8001, so to try it out navigate to

localhost:8001 in a webbrowser

Calculator Test Application: http://localhost:8001/calc/index.html