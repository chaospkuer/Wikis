







用于在c++的const成员函数中改变被mutable修饰的成员变量，比如一个const成员函数用于打印一个成员变量，当需要统计打印次数时，需要将成员变量count设成mutable这样就可以在const的打印函数中打印了。
