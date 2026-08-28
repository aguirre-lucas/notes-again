Add to github on my computer later
```systemverilog
module ff_en_tb #(
	parameter int NUM_TESTS = 10000;
);

	logic clk = 1'b0, rst, en, in, out;
	ff DUT (.*);
	
	initial begin : clk_gen
		forever #5 clk = ~clk;	
	end
	
	initial begin : drive_inputs
		$timeformat(-9, 0, " ns");
		rst <= 1'b1;
		in <= 1'b0;
		en <= 1'b0;
		repeat (5) @(posedge clk);
		@(negedge clk);
		rst <= 1'b0;
		
		for (int i = 0; i < NUM_TESTS; i++) begin
			in <= $urandom;
			en <= $urandom;
		end
		disable clk_gen;
		$display("Tests completed.");
		
	end
	
	// |=> is shorthand for  |-> ##1
	assert property (@(posedge clk) disable iff (rst) en |=> out == $past(in, 1));
	assert property (@(posedge clk) disable iff (rst) out == $past(in, 1, 0)); // might work, we can use $past to get previous values without having to define them and sample them ourselved. i think we can pass in the enable parameter
	// however, this doesn't test the entire system, as we don't test for when the enable isn't asserted
	assert property(@(posedge clk) dsiable iff (rst) !en |=> out == $past(out,1));
	assert property(@(posedge clk) dsiable iff (rst) !en |=> $stable(out));
	// both of these do the same thing where it makes sure the value of out doesn't change if the the enable isn't asserted. 
	assert property (@(posedge clk) rst |=> !out); 
	always @(posedge rst) #1 assert (out == 1'b0);
	// here we can test for when the reset is enabled that 1 clock cycle later, out is 0. I like the assert property more since it's concurrent compared to the sequential always block, but both are equivalent (i think)
	// 

endmodule;	
	

```