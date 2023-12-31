# GolangGuide

## compile, run
* ### Golang is built for simple yet fast compile and executions.   
  * If you have experience with compiling with python virtual environments with requirements.txt or java gradle / maven, 
  you most definately know how complicated and hard it is just to execute a file in a server.


* ### Advantages of golang in aspects of compile / run
  * No language needs to be installed to execute.
  * The result file is a simple executable.
  * simple package management files with .mod files.
  * Can be configured to be built for different OS and CPU architecture.


* ### CMD
  * ### modules
    * #### Init
      * ```shell
        go mod init moduleName
        ```
      * this command creates a go.mod file.   
        The go.mod file contains the module name, dependencies and versions.
    * #### Add dependency
      * ```shell
        go get github.com/pkg/errors
        ```
      * If you run `go get`, this code creates a package in the `go env GOPATH` directory.
        ```shell
        # chekc this path if the package is installed
        go env GOPATH
        ```
      * The go.sum file includes the hash information of the dependency to match the exact version, branch, content.
      * You can also use `go get` to change the version of the used dependency.
        ```shell
        go get github.com/pkg/errors@v0.8.1
        ```
    * #### List dependency
      * ```shell
        go list -m all
        ```
    * #### cleanup dependency
      * ```shell
        go mod tidy
        ```
        If you upgrade or downgrade by specifying the version with `go get`, the go.sum file will not remove the previous 
        version used. This command will remove the previous versions and unused dependencies.
  * ### compile
    * ```shell
      go build mygofile.go
      ```
  * ### compile & run
    * ```shell
      go run mygofile.go
      ```

## types
* ### Unsigned Integers
  * uint (unsigned 64bit or 32 bit by operating system) 
  * uint8 (0 ~ 255)
  * uint16 (0 ~ 65535)
  * uint32 (0 ~ 4294967295)
  * uint64 (0 ~ 18446744073709551615)
* ### Integers
  * int (64bit or 32 bit by operating system)
  * int8 (-128 ~ 127)
  * int16 (-32768 ~ 32767)
  * int32 (-2147483648 ~ 2147483647)
  * int64 (-9223372036854775808 ~ 9223372036854775807)
* ### Floats
  * float (64bit or 32 bit by operating system)
  * float32 (precision of IEEE-754 32 bit)
  * float64 (precision of  IEEE-754 64 bit)
* ### Strings
* ### Booleans
* ### Complex

## fmt
* fmt stands for "format", this package in go scans and prints I/O values by the given format.
* ### Print
  * ```go
    package main
    
    import "fmt"
    
    func main() {
      var printingValue int8 = 32
      // print
      fmt.Print(printingValue)
      // println (print with next line)
      fmt.Println(printingValue)
      // printf (print format) (mostly used)
      fmt.Printf("%d", printingValue)
    }
    ```

* ### Sprint
  * ```go
    package main
    
    import "fmt"
    
    func main() {
        var printingValue int8 = 32
    
        // sprint (set value string from print)
        var settingValue = fmt.Sprintf("%d", printingValue)
        
        fmt.Println(settingValue)
    }
    ```

* ### Scan
  * ```go
    package main
    
    import "fmt"
    
    func main() {
        // scanln (scan line)
        var input string
        fmt.Scanln(&input)
        // scanf (scan by format)
        var integerInput int64
        fmt.Scanf("%d", &integerInput)
    }
    ```

* ### formatting
  * #### value, type
    * `%v` : value
    * `%+v` : struct value with field names
    * `%T` : type
  * #### bool
    * `%t` : bool
  * #### int
    * `%b` : binary
    * `%o` : oxadecimal
    * `%d` : decimal
    * `%x` : hexadecimal
  * #### float
    * `%e` : adds the scientific notation e
    * `%f` : e is removed but lesser decimals
    * `%.2f` : %f with the precision of 0.XX, rounded
    * `%g` : e is removed, for large exponents

  * #### [fmt package for more](https://pkg.go.dev/fmt)


## pointers
* ### Memory stack in Go
  * Every function in go creates a new memory stack in use.
    ![img.png](images/img.png)   
    In this memory stack, all memory usage of variables are considered "immutable".    
    They can only be accessed within the stack and cannot be modified from outside the scope.
    ```go
    package main

    import "fmt"
  
    func main() {
      var IntegerValue int64 = 256
    
      WithoutPointer(IntegerValue)
      fmt.Println(IntegerValue)
    }
    func WithoutPointer(IntegerValue int64) {
      IntegerValue += 10
    }
    ```
    This code will create 2 memory stacks. main and WithoutPointer.   
    Modifying the value of IntegerValue in `WithoutPointer` will not modify the value in `main`.

    ```go
    package main

    import "fmt"
  
    func main() {
      var IntegerValue int64 = 256
    
      WithPointer(&IntegerValue)
      fmt.Println(IntegerValue)
    }
    func WithPointer(IntegerValue *int64) {
      *IntegerValue += 10
    }
    ```
    
    However if you send the variable to the function as a pointer, you can modify memory within different stacks.   
    This is because the `WithPointer` memory stack is given the address of `IntegerValue`.

* ### Usage
  * ### `&Variable`   
    `&` returns the address of of the variables memory. (ex: `0x14000110018`)
  * ### `*VariableMemoryAddress`   
    `*` is used to modify, change or read the variable of the given address.   
    ```go
    var Variable int64 = 0
    var VariableMemoryAddress = &Variable
    *VariableMemoryAddress += 1 
    ```
  * ### `*int64`   
    The `*` sign is also used to tell that the type itself is a pointer by adding it in front of a type.
    ```go
    var Variable int64 = 0
	var VariableMemoryAddress *int64 = &Variable
    ```
    
* ### [Great explenation of pointers in go](https://www.youtube.com/watch?v=sTFJtxJXkaY&t=632s)

## loops
* golang has a similar but different way of defining loops.   
* ### No `while`
  * Golang `for` keyword replaces the `while` keyword unlike most programming languages.   
    ```go
    // while (condition)
    for variable > 10 {
    }
    // while true
    for true {
    }
    ```
* ### Unified `foreach`
  * Golang has a unified method for looping elements from iterations compared to other languages.   
    #### Go iterating array
    ```go
    for i, element := range array {
    }
    ``` 
    #### Go iterate map   
    ```go
    for key, value := range map {
    }
    ```

    #### Java iterate array
    ```
    // available in java 5
    for (Type element : array) {
    }
    ```
    
    #### Java iterate map
    ```
    // 1. using map entryset
    for (Map.Entry<String, Integer> entry : map.entrySet()) {
        entry.getKey();
        entry.getValue();
    }
    
    // 2. using map iterator
    Iterator<Integer> iterator = map.values().iterator();
    while (iterator.hasNext()) {
        Integer value = iterator.next();
    }
    
    // 3. using map foreach
    map.forEach((key, value) -> ...);
    ```   
    as you can see, Golang does have a more unified and simple method compared to java.

## Error Handling
* golang has quite a unique way of handling errors compared to other languages.
* ### panic
  * Conventional programming languages throw exceptions, in Go, we throw panics.   
    But as funny as it seems, we cannot catch a panic like a try catch method.
  * #### The characteristics of panic
    1. The goroutines where the panic occurred terminates
    2. It does not affect the execution of other goroutines
    3. It is a unrecoverable state and terminate immediately
* ### handling panic with `defer` and `recover`
  * I wouldn't call this "handling" because `defer` will not stop the termination and keep the goroutine running. 
    But still we should be able to log the panic and change our code to return it as a custom error afterwards.      
    ```go
    func main() {
        defer func() {
            if r := recover(); r != nil {
                fmt.Println("defer check for from panic:", r)
            }
        }()

        panic("panic attack")
    }
    ```   
    Here, the `r:=recover();` returns the string "panic attack".   
    Remember that panic has a parameter of type `any`, so `recover` returns whatever type that is given.
* ### handling goroutines panic with `defer`
  * When we use goroutines, we use `WaitGroup` in order to wait for the routine to be finished.    
    If the routine panics without `wg.Done()`, the program will wait forever. In order to block such behaviour we must `defer`.
  * ```go
    var wg sync.WaitGroup
  
    func waitRoutine() {
      defer wg.Done()
      panic("panic attack")
    }
  
    func main() {
      wg.Add(1)
      go waitRoutine()
      wg.Wait()
    }
    ```
* ### errors
  * If you check the `errors.go` package, you can see that it is shockingly simple.   
    But because of it's "short" code, it does lack some functionality that other languages offer as a given.   
    One of the features that lack I find annoying is getting the stack trace.
    ![img.png](images/img2.png)
  * #### The lack of information in errors   
    In java, the exception class itself has a `printStackTrace`.   
    ```
    try {
        throw new RuntimeException("oops, error.");
    } catch (RuntimeException e) {
        e.printStackTrace();
    }
    ```
    Creating custom errors might me sufficient enough, but if you use that custom error raised in two different places, how would 
    you debug just using a the string message?   
    So our best bet would be to create a custom error struct that contains a stack trace.
    ```go
    package main
    
    import (
        "fmt"
        "runtime/debug"
    )
    
    type CustomError struct {
        errorMessage string
        stack        string
    }
    
    func (e CustomError) Error() string {
        return e.errorMessage
    }
    
    func NewCustomError(message string) CustomError {
        return CustomError{errorMessage: message, stack: string(debug.Stack())}
    }
    
    func main() {
        err := NewCustomError("oops, error.")
        fmt.Println(err.stack)
    }
    ```
    If you use custom logging services like datadog or logstash, you should customize your error struct to be as specific as possible
    , such as the name of the gorutine `runtime.GoID()`, the current executing code file with `runtime.Caller(1)` etc.   
    <br/>
    Example of error logging in datadog
    ![img.png](img.png)
    