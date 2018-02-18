---
title: "Golang Dependency Injection With karlkfi/inject"
date: 2018-02-18T19:14:10+01:00
categories: ["Blog" ]
tags: ["go", "dependency injection"]
---

While investigating the pros and cons of manual ctor and IoC based dependency injection 
in [go][golang] I looked at several IoC solutions:


* [karlkfi/inject](https://github.com/karlkfi/inject)
* [facebookgo/inject](https://github.com/facebookgo/inject)
* [magic003/alice](https://github.com/magic003/alice)

I was astonished by [karlkfi/inject](https://github.com/karlkfi/inject) how I was
able to switch to an IoC with **NO CHANGES WHATSOVER** on my ctor DI based classes.

Check out the small changes yourself below or on [github](https://github.com/ckolumbus/golangRestApiExampleWithDependencyInjection/commit/0c43952092e1f313e1a73a0c3cb938fed595e386#diff-77339446d6d7a72280ed646533aa22ae)

[golang]: http://golang.com

<!--more-->

Main features for me:

* no class attributes necessary
* Checkout `NewAutoProvider` for completely automatic constructor call creation
* Checkout `NewProvider` for allowing to pass explicit parameters (like the `db` context)


{{< highlight diff >}}

@@ -21,6 +21,7 @@
 package main
 
 import (
+	"github.com/karlkfi/inject"
 	"github.com/labstack/echo"
 
 	"github.com/ckolumbus/golangRestApiExampleWithDependencyInjection/pkg/employee/controller"
@@ -32,13 +33,20 @@ import (
 )
 
 func main() {
+	var (
+		employeePersist    persistence.IEmployeePersist
+		employeeController *controller.EmployeeController
+	)
 	conn := db.SetupDB("sqlite3", "./db.sqlite")
 	defer conn.Close()
 
 	e := echo.New()
 
-	employeePersist := persistence.NewEmployeePersist(conn)
-	employeeController := controller.NewEmployeeController(employeePersist)
+	graph := inject.NewGraph(
+		inject.NewDefinition(&employeePersist, inject.NewProvider(persistence.NewEmployeePersist, &conn)),
+		inject.NewDefinition(&employeeController, inject.NewAutoProvider(controller.NewEmployeeController)),
+	)
+	graph.Resolve(&employeeController)
 
 	e.POST("/employee", employeeController.CreateEmployee)
 	e.DELETE("/employee/:id", employeeController.DeleteEmployee)

{{< /highlight >}}