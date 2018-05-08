# try-catch-finally+return总结

**无论发生异常与否，finally语句块一定在return之前执行**。

以下结论基于如下前提：try的return语句在发生异常语句之后、方法有返回值。
1. 当finally语句块中存在return语句时，此时无论是否发生异常，都以该return语句为准，其他的return语句将不起作用。
2. 当发生异常时，try存在return语句，catch存在return语句，以catch中的return语句为准，此时在finally语句块中的修改将不起作用。
3. 当发生异常时，try存在return语句，catch不存在return语句，方法在最后存在return语句，以该return语句为准，此时在catch和finally的修改会起作用。
4. 不发生异常时，try存在return语句，不管是catch存在return语句还是方法最后存在return语句，都以try中的return语句为准，此时在catch、finally中的修改将不起作用。