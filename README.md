# libtcc bindings for Clay

## Instructions
Link with `-ldl` and `-ltcc`.

## Example
<pre>
import tcc.*;

main() {
    // A string holding the C program we will Just-In-Time compile with TCC.
	var program = "
    int fib(int n) {
		if(n <= 2)
			return 1;
		else
			return fib(n-1) + fib(n-2);
	}

	int foo(int n) {
		printf(\"Hello world!\\n\");
		printf(\"fib(%d) = %d\\n\", n, fib(n));
		printf(\"add(%d, %d) = %d\\n\", n, 2 * n, add(n, 2 * n));
		return 0;
	}";
    
    // Create a new TCC compilation context and compile the above C program.
	var tcc = TCC();
	compileString(tcc, program);

    // Add Clay's "+" operator visible to the C code as "add".
	addFunction(tcc, "add", add, Int, Int);

    // You can't get functions and symbols from the compiled code until you relocate.
    relocate(tcc);

    // Get and call the "foo" function from the C program.
	var func = getFunction(tcc, "foo", (Int), (Int));
	func(32);
}
</pre>

