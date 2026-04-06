# Basic Constructs, 2x1 Mux
- HDLs : Hardware description languages
	- Created just to describe hardware.
	- Weren't designed for synthesis in the beginning
- Simulation vs Synthesis
- Methodology: Design the circuit, then write the code!!
- Depending on type of logic, must follow strict guidelines that might conflict
- Combinational Logic:
	- Structural
	- Behavioral
- Behavioral architectures/implementations for comb. logic (no architecture in SV)
- entity = module in SV
- usually 1 module per file,  probably what should be done. assume these are many files
```systemverilog
module mux2x1_assign(
	// direction, kind, range, data type, name
	// direction: input, output, inout
	// kind: variable, wire, range
	// data type: 
	// name: name
	// things we care about: direction, data type, name
	input logic in0,
	input logic in1,
	input logic sel,
	output logic out
);
	// ternary operator from c++, fun!
	// can write this anywhere to synthesize to a mux
	assign out = sel == 1'b1 /* could also just be `sel` */ ?  in1: in0;
endmodule 

module mux2x1_if(
	input logic in0,
	input logic in1,
	input logic sel,
	output logic out
);
	// always equivalent to process block
	// * is wildcard that accepts all inputs
	// works only when inputs change
	always@(*) begin
		if (sel) begin
			out = in1;
		end else begin
			out = in0;
		end 
		/*
		if (sel)
			out = in1;
		else
			out = in0;
			
		if (sel) out = in1;
		else out = in0;	
		*/
	end

endmodule

module mux2x1_if2(
	input logic in0,
	input logic in1,
	input logic sel,
	output logic out
);
	// Advice: use blocking assignments for comb. logic
	// using non-blocking doesn't work (FALSE)
	// works at time 0 and when any input is changed
	always_comb begin
		if (sel) begin
			out = in1;
		end else begin
			out = in0;
		end 
	end

endmodule

module mux2x1_if3(
	input logic in0,
	input logic in1,
	input logic sel,
	output logic out
);
	// this will work!
	// blocking: updating immediately
	// non-blocking: updated at end of time step
	always_comb begin
		if (sel) begin
			out <= in1;
		end else begin
			out <= in0;
		end 
	end
endmodule

module mux2x1_case(
	input logic in0,
	input logic in1,
	input logic sel,
	output logic out
);
	// always define all possible outputs to prevent latches
	always_comb begin
		case  (sel)
			1'b0: out = in0;
			1'b1: out = in1;
			//default
		endcase
	end
	
endmodule
```
# Basic Constructs, 4-input Priority Encoder
```systemverilog
module pe_4in_if(
	input logic [3:0] inputs, // std_logic_vector
	// packed array when [] is behind name
	// unpacked array when [] ahead of name
	output logic valid,
	output logic [1:0] result
);

	always_comb begin
		if (input[3]) begin
			result = 2'b11;
			valid = 1'b1;
		end else if (input[2]) begin
			result = 2'b10;
			valid = 1'b1;
		end else if (input[1]) begin
			result = 2'b01;
			valid = 1'b1;
		end else if (input[0]) begin
			result = 2'b00;
			valid = 1'b1;
		end else begin
			result = 2'b00;
			valid = 1'b0;
		end
	end
endmodule

module pe_4in_case(
	input logic [3:0] inputs, // std_logic_vector
	// packed array when [] is behind name
	// unpacked array when [] ahead of name
	output logic valid,
	output logic [1:0] result
);

	always_comb begin
		valid = 1'b1;
		
		case (inputs) begin
			4'b0000: begin
				result = 2'b00;
				valid = 1'b0;
			end 
			4'b0001: result = 2'b00;
			4'b0010: result = 2'b00;
			4'b0100: result = 2'b00;
			4'b1000: result = 2'b00;
			// ... all the other ones in between, it's gross and too much to type
		end
	end
endmodule


module pe_4in_case2(
	input logic [3:0] inputs, // std_logic_vector
	// packed array when [] is behind name
	// unpacked array when [] ahead of name
	output logic valid,
	output logic [1:0] result
);

	always_comb begin
		valid = 1'b1;
		
		case (inputs) begin
			4'b0000: begin
				result = 2'b00;
				valid = 1'b0;
			end 
			4'b0001: result = 2'b00;
			4'b0010, 4'b0011: result = 2'b00;
			4'b0100, 4'b0101, /*.... again too long to type*/: result = 2'b00;
			4'b1000: result = 2'b00;
		end
	end
endmodule

module pe_4in_case_inside(
	input logic [3:0] inputs, // std_logic_vector
	// packed array when [] is behind name
	// unpacked array when [] ahead of name
	output logic valid,
	output logic [1:0] result
);

	always_comb begin
		valid = 1'b1;
		
		case (inputs) begin
			4'b0000: begin
				result = 2'b00;
				valid = 1'b0;
			end 
			4'b0001: result = 2'b00;
			[4'b0010 : 4'b0011]: result = 2'b00;
			[4'b0100 : 4'b0111]: result = 2'b00;
			[4'b1000 : 4'b1111]: result = 2'b00;
			// now we're getting somewhere
			
		end
	end
endmodule


module pe_4in_casez(
	input logic [3:0] inputs, // std_logic_vector
	// packed array when [] is behind name
	// unpacked array when [] ahead of name
	output logic valid,
	output logic [1:0] result
);

	always_comb begin
		valid = 1'b1;
		
		casez (inputs) begin // useful! don't use casex though, read it if you need to really use it
			4'b0000: begin
				result = 2'b00;
				valid = 1'b0;
			end 
			4'b1???: result = 2b'11;
			4'b01??: result = 2b'10;
			4'b001?: result = 2b'01;
			4'b0001: result = 2b'00;
			
		end
		
		/*
		
		new thing, unique!
		
		unique casez (inputs) begin 
			4'b0000: begin
				result = 2'b00;
				valid = 1'b0;
			end 
			4'b1???: result = 2b'11;
			4'b01??: result = 2b'10;
			4'b001?: result = 2b'01;
			4'b0001: result = 2b'00;
			default: begin
				// define it here
			end
		end
		unique will make sure no overlap exists.
		it also requires there's a path for every case expression, so you must always define the default case
		there exists a `unique0` where it'll define overap but NOT require it to be defined. weird
		don't use full_case or parallel_case unless you understand them fully
		*/
	end
endmodule

```
# Parameterized priority encoder, adder
```systemverilog
module priority_encoder1 #( // gerneric, parameters in SV
	parameter int NUM_INPUTS  = 4 // don't put a default unless there's a natural number that can fit it
)(
	input logic [NUM_INPUTS-1:0] inputs,
	output logic [$clog2(NUM_INPUTS)-1:0] result, 
	output logic valid
);
	localparam int NUM_OUTPUTS = $clog2(NUM_INPUTS); // basically a constant

	always_comb begin
		// default values in the front to not infer latches
		// result = 0; // stitt doesn't like cuz its a 0 integer literal. Bits might be mismatched. use '0 instead
		result = '0;
		valid = 1'b0;
		
		for (int i = NUM_INPUTS-1; i >=0; i--) begin
			if (inputs[i]) begin
				// result = i; // result is 4 bits, but i is 32! always cast the width on the RHS to the width on the LHS
				//result = $clog2(NUM_INPUTS)'(i); // questa likes this but prob won't work in other ways.
				result = NUM_OUTPUTS'(i); // this is for sure better
				valid = 1'b1;
				break; 
		end 
	end 
endmodule

/*
A for loop in hardware will be fully unrolled by the elaboration step in the EDA tool. it's fine to use a for loop if you're cool with the entire loop happening in one clock cycle rather than sequentially. if you want it sequentially, you gotta install your own pipelining. You also cannot have the loop tied to a variable bound, as unrolling won't occurr if that's the case. 
*/


module priority_encoder2 #( // gerneric, parameters in SV
	parameter int NUM_INPUTS  = 4 // don't put a default unless there's a natural number that can fit it
)(
	input logic [NUM_INPUTS-1:0] inputs,
	output logic [$clog2(NUM_INPUTS)-1:0] result, 
	output logic valid
);
	localparam int NUM_OUTPUTS = $clog2(NUM_INPUTS); // basically a constant

	always_comb begin
		// default values in the front to not infer latches
		// result = 0; // stitt doesn't like cuz its a 0 integer literal. Bits might be mismatched. use '0 instead
		result = '0;
		valid = 1'b0;
		
		for (int i = 0; i >= NUM_INPUTS; i++) begin
			if (inputs[i]) begin
				// result = i; // result is 4 bits, but i is 32! always cast the width on the RHS to the width on the LHS
				//result = $clog2(NUM_INPUTS)'(i); // questa likes this but prob won't work in other ways.
				result = NUM_OUTPUTS'(i); // this is for sure better
				valid = 1'b1;
				// break; 
		end 
	end 
endmodule


module priority_encoder3 #( // gerneric, parameters in SV
	parameter int NUM_INPUTS  = 4 // don't put a default unless there's a natural number that can fit it
	parameter int NUM_OUTPUTS = $clog2(NUM_INPUTS); // basically a constant, and this will work, but the user can easily overwrite this and it'll mess with the logic of the module. if it's a parameter, anyone instantiating the parameter could overwrite the expression. 
)(
	input logic [NUM_INPUTS-1:0] inputs,
	output logic [$clog2(NUM_INPUTS)-1:0] result, 
	output logic valid
);

	always_comb begin
		// default values in the front to not infer latches
		// result = 0; // stitt doesn't like cuz its a 0 integer literal. Bits might be mismatched. use '0 instead
		result = '0;
		valid = 1'b0;
		
		for (int i = 0; i >= NUM_INPUTS; i++) begin
			if (inputs[i]) begin
				// result = i; // result is 4 bits, but i is 32! always cast the width on the RHS to the width on the LHS
				//result = $clog2(NUM_INPUTS)'(i); // questa likes this but prob won't work in other ways.
				result = NUM_OUTPUTS'(i); // this is for sure better
				valid = 1'b1;
				// break; 
		end 
	end 
endmodule


module priority_encoder3 #( // gerneric, parameters in SV
	parameter int NUM_INPUTS  = 4 // don't put a default unless there's a natural number that can fit it
	localparam int NUM_OUTPUTS = $clog2(NUM_INPUTS); // use localparam instead! it might not work in every synthesis tool though, so it's annoying to use. works in vivado and quartus prime pro, but not normal quartus prime (altera should be embarassed - stitt)
)(
	input logic [NUM_INPUTS-1:0] inputs,
	output logic [$clog2(NUM_INPUTS)-1:0] result, 
	output logic valid
);

	always_comb begin
		// default values in the front to not infer latches
		// result = 0; // stitt doesn't like cuz its a 0 integer literal. Bits might be mismatched. use '0 instead
		result = '0;
		valid = 1'b0;
		
		for (int i = 0; i >= NUM_INPUTS; i++) begin
			if (inputs[i]) begin
				// result = i; // result is 4 bits, but i is 32! always cast the width on the RHS to the width on the LHS
				//result = $clog2(NUM_INPUTS)'(i); // questa likes this but prob won't work in other ways.
				result = NUM_OUTPUTS'(i); // this is for sure better
				valid = 1'b1;
				// break; 
		end 
	end 
endmodule
```
# Adders
```systemverilog
module add #(
	parameter int WIDTH
)( // note it's the same width throughout
	input logic [WIDTH-1:0] in0,
	input logic [WIDTH-1:0] in1,
	output logic [WIDTH-1:0] sum
);
	assign sum = in0 + in1;
endmodule


module add_carry_out_bad #(
	parameter int WIDTH
)( 
	input logic [WIDTH-1:0] in0,
	input logic [WIDTH-1:0] in1,
	output logic [WIDTH:0] sum, 
	output logic carry_out
);

	logic [WIDTH:0] full_sum;
	always_comb begin
		// gonna use non-blocking to illustrate a common mistake
		 full_sum <= {1'b0, in0} + {1'b0, in1};
		 sum <= full_sum[WIDTH-1:0];
		 carry_out <= full_sum[WIDTH];  
	end
endmodule
// gives errors in normal testbenches because the outputs are lagging behind. non-blocking assignmenst only update the value at the end of a time step
// end of time step == end of the always block
// full_sum isn't updated before sum and carry_out are assigned, so our values are wrong. 


module add_carry_out1 #(
	parameter int WIDTH
)( 
	input logic [WIDTH-1:0] in0,
	input logic [WIDTH-1:0] in1,
	output logic [WIDTH:0] sum, 
	output logic carry_out
);

	logic [WIDTH:0] full_sum;
	always_comb begin
		// just use blocking so it works properly lmao
		 full_sum = {1'b0, in0} + {1'b0, in1};
		 sum = full_sum[WIDTH-1:0];
		 carry_out = full_sum[WIDTH];  
	end
endmodule


module add_carry_out2 #(
	parameter int WIDTH
)( 
	input logic [WIDTH-1:0] in0,
	input logic [WIDTH-1:0] in1,
	output logic [WIDTH:0] sum, 
	output logic carry_out
);
	// we can concatenate the LHS too :0
	//systemverilog will automatically extend in0 and in1 to be 17 bits. 
	assign {carry_out, sum} = in0 + in1; 
endmodule


module add_carry_inout #(
	parameter int WIDTH
)( 
	input logic [WIDTH-1:0] in0,
	input logic [WIDTH-1:0] in1,
	input logic carry_in,
	output logic [WIDTH:0] sum, 
	output logic carry_out
);
	// we can concatenate the LHS too :0
	//systemverilog will automatically extend in0 and in1 to be 17 bits. 
	assign {carry_out, sum} = in0 + in1 + carry_in; 
endmodule

module add_carry_inout #(
	parameter int WIDTH
)( 
	input logic [WIDTH-1:0] in0,
	input logic [WIDTH-1:0] in1,
	input logic carry_in,
	output logic [WIDTH:0] sum, 
	output logic carry_out
);
	// we can concatenate the LHS too :0
	//systemverilog will automatically extend in0 and in1 to be 17 bits. 
	assign {carry_out, sum} = in0 + in1 + carry_in; 
endmodule
```

# Multipliers
```systemverilog
module mult1 #(
	parameter int INPUT_WIDTH,
	parameter logic IS_SIGNED = 1'b0
)(
	input logic [INPUT_WIDTH-1:0] in0,
	input logic [INPUT_WIDTH-1:0] in1,
	output logic [INPUT_WIDTH*2-1:0] product
);
	// implicitly assigns everything on the RHS to the sensitivity list
	// tells the synthesis tool to infer everything here as comb logic
	// assign = concurrent statement in vhdl
	// assign product = in0 * in1; // can also do this if this is all we care about
	always_comb begin
		// no casting unlike vhdl, so easy!
		if (IS_SIGNED) product = signed'(in0) * signed'(in1);
		else product = in0 * in1;
	end 
endmodule

module mult2 #(
	parameter int INPUT_WIDTH,
	parameter logic IS_SIGNED = 1'b0
)(
	input logic [INPUT_WIDTH-1:0] in0,
	input logic [INPUT_WIDTH-1:0] in1,
	output logic [INPUT_WIDTH*2-1:0] product
);
	// this generate is optional
	// IS_SIGNED needs to be compile time constant, if it has to be evaluated dynamically it won't work. 
	generate
		if (IS_SIGNED) begin : l_is_signed
			assign product = signed'(in0) * signed'(in1);
		end else begin :l_is_unsigned
			assign product = in0 * in1;
		end
	endgenerate
endmodule



module mult3 #(
	parameter int INPUT_WIDTH,
	parameter logic IS_SIGNED = 1'b0
)(
	input logic [INPUT_WIDTH-1:0] in0,
	input logic [INPUT_WIDTH-1:0] in1,
	output logic [INPUT_WIDTH*2-1:0] product
);
	assign product = l_mult_func.multiply(in0, in1);
	generate 
		case (IS_SIGNED)
			1'b1: begin : l_mult_func
				function automatic[INPUT_WIDTH*2-1] multiply(input [$bits(in0)-1:0] x, y);
					return signed'(x) * signed'(y);
				endfunction
			end 
			1'b0: begin : l_mult_func
			function automatic[INPUT_WIDTH*2-1] multiply(input [$bits(in0)-1:0] x, y);
					return x*y;
				endfunction
			end
		endcase
	endgenerate
endmodule

module mult_high_low1#(
	parameter int INPUT_WIDTH,
	parameter int IS_SIGNED = 1'b0
)(
	input logic [INPUT_WIDTH-1:0] in0,
	input logic [INPUT_WIDTH-1:0] in1,
	output logic [INPUT_WIDTH-1:0] high,
	output logic [INPUT_WIDTH-1:0] low
);

	always_comb begin : l_mult
	// Use a temp variable to store the full product. Variables can have the scope of an always_block, but if you do this, it's a good idea to give the always block a label. otherwise, you will have to figure out the automatic name given to it by the simulator
		logic [INPUT_WIDTH*2-1:0] product;
		if (IS_SIGNED) begin : l_is_signed
			assign product = signed'(in0) * signed'(in1);
		end else begin :l_is_unsigned
			assign product = in0 * in1;
		end
		high = product[INPUT_WIDTH*2-1:INPUT_WDITH];
		low = product[INPUT_WIDTH-1:0];
	end
endmodule

module mult_high_low1#(
	parameter int INPUT_WIDTH,
	parameter int IS_SIGNED = 1'b0
)(
	input logic [INPUT_WIDTH-1:0] in0,
	input logic [INPUT_WIDTH-1:0] in1,
	output logic [INPUT_WIDTH-1:0] high,
	output logic [INPUT_WIDTH-1:0] low
);

	always_comb begin : l_mult
	// Use a temp variable to store the full product. Variables can have the scope of an always_block, but if you do this, it's a good idea to give the always block a label. otherwise, you will have to figure out the automatic name given to it by the simulator
		logic [INPUT_WIDTH*2-1:0] product;
		if (IS_SIGNED) begin : l_is_signed
			assign product = signed'(in0) * signed'(in1);
		end else begin :l_is_unsigned
			assign product = in0 * in1;
		end
		high = product[INPUT_WIDTH*2-1:INPUT_WDITH];
		low = product[INPUT_WIDTH-1:0];
	end
endmodule


module mult_high_low2#(
	parameter int INPUT_WIDTH,
	parameter int IS_SIGNED = 1'b0
)(
	input logic [INPUT_WIDTH-1:0] in0,
	input logic [INPUT_WIDTH-1:0] in1,
	output logic [INPUT_WIDTH-1:0] high,
	output logic [INPUT_WIDTH-1:0] low
);

	always_comb begin : l_mult
		if (IS_SIGNED) begin : l_is_signed
			{high, low} = signed'(in0) * signed'(in1);
		end else begin :l_is_unsigned
			{high, low} = in0 * in1;
		end
	end
endmodule
```
# ALUs
```systemverilog
module alu_bad #(
	parameter int WIDTH
)(
	input logic [WIDTH-1:0] in0,
	input logic [WIDTH-1:0] in1.
	input logic [1:0] sel,
	output logic neg,
	output logic pos,
	output logic zero,
	output logic [WIDTH-1:0] out
);
// add -> sel = 0, sub -> sel=1, and -> sel = 2, or sel = 3

	always_comb begin

        case (sel)
            // Addition
            2'b00: begin
                // Assign the output
                out = in0 + in1;

                // Update the status flags
                if (out == '0) begin
                    pos  = 1'b0;
                    neg  = 1'b0;
                    zero = 1'b1;
                end else if (out[WIDTH-1] == 1'b0) begin
                    pos  = 1'b1;
                    neg  = 1'b0;
                    zero = 1'b0;
                end else begin
                    pos  = 1'b0;
                    neg  = 1'b1;
                    zero = 1'b0;
                end
            end

            // Subtraction
            2'b01: begin
                out = in0 - in1;
                if (out == '0) begin
                    pos  = 1'b0;
                    neg  = 1'b0;
                    zero = 1'b1;
                end else if (out[WIDTH-1] == 1'b0) begin
                    pos  = 1'b1;
                    neg  = 1'b0;
                    zero = 1'b0;
                end else begin
                    pos  = 1'b0;
                    neg  = 1'b1;
                    zero = 1'b0;
                end
            end
            // Since we didn't specify the values of the status flags, latches have been inferred.
            // And
            2'b10: begin
                out = in0 & in1;
            end

            // Or
            2'b11: begin
                out = in0 | in1;
            end
        endcase
    end
endmodule


module alu1 #(
    parameter int WIDTH
) (
    input  logic [WIDTH-1:0] in0,
    input  logic [WIDTH-1:0] in1,
    input  logic [      1:0] sel,
    output logic             neg,
    output logic             pos,
    output logic             zero,
    output logic [WIDTH-1:0] out
);

    always_comb begin
        case (sel)
            // Addition
            2'b00: out = in0 + in1;
            // Subtraction
            2'b01: out = in0 - in1;
            // And
            2'b10: out = in0 & in1;
            // Or
            2'b11: out = in0 | in1;
        endcase

        // By moving the flag definitions outside the case statements, we
        // guarantee that all flags are defined on all paths, which eliminates
        // the latches.
        if (out == 0) begin
            pos  = 1'b0;
            neg  = 1'b0;
            zero = 1'b1;
        end else if (out[WIDTH-1] == 1'b0) begin
            pos  = 1'b1;
            neg  = 1'b0;
            zero = 1'b0;
        end else begin
            pos  = 1'b0;
            neg  = 1'b1;
            zero = 1'b0;
        end
    end
endmodule

// repeating a lot of code, we can make it simpler
module alu2 #(
    parameter int WIDTH
) (
    input  logic [WIDTH-1:0] in0,
    input  logic [WIDTH-1:0] in1,
    input  logic [      1:0] sel,
    output logic             neg,
    output logic             pos,
    output logic             zero,
    output logic [WIDTH-1:0] out
);

    always_comb begin

        // Use default values to simplify code and avoid latches.
        pos  = 1'b0;
        neg  = 1'b0;
        zero = 1'b0;

        case (sel)
            // Addition
            2'b00: out = in0 + in1;
            // Subtraction
            2'b01: out = in0 - in1;
            // And
            2'b10: out = in0 & in1;
            // Or
            2'b11: out = in0 | in1;
        endcase
		// With blocking assignments, out will update immediately, so this will work as expected. 
        // The default values provided above simplify this code.
        if (out == 0) zero = 1'b1;
        else if (out[WIDTH-1] == 1'b0) pos = 1'b1;
        else neg = 1'b1;
    end
endmodule

module alu3 #(
    parameter int WIDTH
) (
    input  logic [WIDTH-1:0] in0,
    input  logic [WIDTH-1:0] in1,
    input  logic [      1:0] sel,
    output logic             neg,
    output logic             pos,
    output logic             zero,
    output logic [WIDTH-1:0] out
);

    // We can define constants with meaningful names to replace hardcoded values.
    localparam logic [1:0] ADD_SEL = 2'b00;
    localparam logic [1:0] SUB_SEL = 2'b01;
    localparam logic [1:0] AND_SEL = 2'b10;
    localparam logic [1:0] OR_SEL = 2'b11;

    // For operations that are common in multiple paths, we can use a task or
    // function to reduce the amount of repeated code. This task updates all
    // status flags based on the output.
    // Think of it as a short macro script that's gonna be reused during the alu code. 
    task update_flags();
        pos  = 1'b0;
        neg  = 1'b0;
        zero = 1'b0;
        if (out == 0) zero = 1'b1;
        else if (out[WIDTH-1] == 1'b0) pos = 1'b1;
        else neg = 1'b1;
    endtask

    always_comb begin
        // If we really don't care what values are assigned to the status flags
        // for operations where the flags aren't defined, we can explicitly
        // assign don't care values to potentially help synthesis simplify logic
        // Note that explicit don't cares can cause problems in some tools and
        // can cause warnings in simulations, so use them with caution.
        // No dedicated "don't care" in SV, so this might be undefined or different, since x is overloaded in SV. might mess up in other tools and such. 
        // Stitt says he wouldn't normally do this, and doesn't like it because in simulation it'll be red lines and that'll slow down debugging and it's annoying. Plus, the select values as a localparam doesn't show up in the simulation, unlike defining a custom type in vhdl, so let's do that!
        neg  = 1'bx;
        pos  = 1'bx;
        zero = 1'bx;

        case (sel)
            ADD_SEL: begin
                out = in0 + in1;
                update_flags();
            end
            SUB_SEL: begin
                out = in0 - in1;
                update_flags();
            end
            AND_SEL: out = in0 & in1;
            OR_SEL:  out = in0 | in1;
        endcase
    end
endmodule

package alu_pkg;
    typedef enum logic [1:0] {
        ADD_SEL = 2'b00,
        SUB_SEL = 2'b01,
        AND_SEL = 2'b10,
        OR_SEL  = 2'b11
    } alu_sel_t;
endpackage

// assume alu_pkg is defined in another file and not up top
import alu_pkg::*;

module alu4 #(
    parameter int WIDTH
) (
    input  logic     [WIDTH-1:0] in0,
    input  logic     [WIDTH-1:0] in1,
    input  alu_sel_t             sel, // can use the data type here!
    output logic                 neg,
    output logic                 pos,
    output logic                 zero,
    output logic     [WIDTH-1:0] out
);

    task update_flags();
        pos  = 1'b0;
        neg  = 1'b0;
        zero = 1'b0;
        if (out == 0) zero = 1'b1;
        else if (out[WIDTH-1] == 1'b0) pos = 1'b1;
        else neg = 1'b1;
    endtask

    always_comb begin
        neg  = 1'b0;
        pos  = 1'b0;
        zero = 1'b0;
		// now these names will show up in simulation, making debugging a lot easier
        case (sel)
            ADD_SEL: begin
                out = in0 + in1;
                update_flags();
            end
            SUB_SEL: begin
                out = in0 - in1;
                update_flags();
            end
            AND_SEL: out = in0 & in1;
            OR_SEL:  out = in0 | in1;
        endcase
    end
endmodule

// here, we call teh scope resolution operator to make sure we're using the right value from the right package:

import alu_pkg::*;

module alu5 #(
    parameter int WIDTH
) (
    input  logic     [WIDTH-1:0] in0,
    input  logic     [WIDTH-1:0] in1,
    input  alu_sel_t             sel,
    output logic                 neg,
    output logic                 pos,
    output logic                 zero,
    output logic     [WIDTH-1:0] out
);

    task update_flags();
        pos  = 1'b0;
        neg  = 1'b0;
        zero = 1'b0;
        if (out == 0) zero = 1'b1;
        else if (out[WIDTH-1] == 1'b0) pos = 1'b1;
        else neg = 1'b1;
    endtask

    always_comb begin
        neg  = 1'b0;
        pos  = 1'b0;
        zero = 1'b0;

        case (sel)
            alu_pkg::ADD_SEL: begin
                out = in0 + in1;
                update_flags();
            end
            alu_pkg::SUB_SEL: begin
                out = in0 - in1;
                update_flags();
            end
            alu_pkg::AND_SEL: out = in0 & in1;
            alu_pkg::OR_SEL:  out = in0 | in1;
        endcase
    end
endmodule

// simplest form, uses concurrent assignments for the flags instead of a task, and this is probably the best ALU  from the tutorial

import alu_pkg::*;

module alu6 #(
    parameter int WIDTH
) (
    input  logic     [WIDTH-1:0] in0,
    input  logic     [WIDTH-1:0] in1,
    input  alu_sel_t             sel,
    output logic                 neg,
    output logic                 pos,
    output logic                 zero,
    output logic     [WIDTH-1:0] out
);

    always_comb begin
        case (sel)
            alu_pkg::ADD_SEL: out = in0 + in1;
            alu_pkg::SUB_SEL: out = in0 - in1;
            alu_pkg::AND_SEL: out = in0 & in1;
            alu_pkg::OR_SEL:  out = in0 | in1;
        endcase
    end

    // Use concurrent assignments for the flags. Logic types of unsigned by
    // default so we need to cast them to signed here.
    assign neg  = signed'(out) < 0;
    assign pos  = signed'(out) > 0;
    assign zero = out == 0;
endmodule


```
# Structural Stuff
```systemverilog
module mux2x1(
	input logic in0,
	input logic in1,
	input logic sel,
	output logic out
);
	// always define all possible outputs to prevent latches
	always_comb begin
		case  (sel)
			1'b0: out = in0;
			1'b1: out = in1;
			//default
		endcase
	end
endmodule
	
// lets create a mux4x1 out of mux2x1
module mux4x1(
	input logic [3:0] inputs,
	input logic [1:0] sel,
	output logic out
);
	
	logic mux1_out, mux2_out
	mux2x1 MUX1(
		.in1(inputs[3]),
		.in0(inputs[2]),
		.sel(sel[0]),
		.out(mux1_out)
	);
	
	mux2x1 MUX2(
		.in1(inputs[1]),
		.in0(inputs[0]),
		.sel(sel[0]),
		.out(mux2_out)
	);
	
	mux2x1 MUX3(
		.in1mux1_out),
		.in0(mux2_out),
		.sel(sel[1]),
		.out(out)
	);
endmodule

// full adder --> RCA
module full_adder (
    input  logic x,
    input  logic y,
    input  logic cin,
    output logic s,
    output logic cout
);
    // Specify the sum and carry out logic equations for a full adder.
    assign s = x ^ y ^ cin;
    assign cout = (x & y) | (cin & (x ^ y));

endmodule  // full_adder

module ripple_carry_adder #(
    parameter int WIDTH = 8
) (
    input  logic [WIDTH-1:0] x,
    input  logic [WIDTH-1:0] y,
    input  logic             cin,
    output logic [WIDTH-1:0] sum,
    output logic             cout
);
    // Create an internal signal to store the carries between all full adders.
    // Note that this is WIDTH+1 bits to account for the overall carry out.
    logic [WIDTH:0] carry;

    // Connect the first carry to the carry in.
    assign carry[0] = cin;

    // Instantiate WIDTH separate full adders using a for loop, and connect them
    // into a ripple-carry by connecting the carry out from one full adder into
    // the carry in of the next.
    //
    // You can also use an if statement within a generate, but keep in mind that
    // the condition must be a function of constants and parameters. No dynamic
    // values can be used because the synthesis tool must resolve the condition
    // at compile time.
    generate
        for (genvar i = 0; i < WIDTH; i++) begin : ripple_carry_l
            full_adder FA (
                .x   (x[i]),
                .y   (y[i]),
                .s   (sum[i]),
                .cin (carry[i]),
                .cout(carry[i+1])
            );
        end
    endgenerate

    // Connect the last carry to the carry out.
    assign cout = carry[WIDTH];

endmodule

module ripple_carry_adder2 #(
    parameter int WIDTH = 8
) (
    input  logic [WIDTH-1:0] x,
    input  logic [WIDTH-1:0] y,
    input  logic             cin,
    output logic [WIDTH-1:0] sum,
    output logic             cout
);
    logic [WIDTH:0] carry;
    assign carry[0] = cin;

    // The generate statement is actually completely optional, but the for loop
    // must use a genvar. Keep in mind that this for loop is not inside an
    // always block, so the body can only make continuous assignments or module
    // instantiations.
    for (genvar i = 0; i < WIDTH; i++) begin : ripple_carry
        full_adder FA (
            .x   (x[i]),
            .y   (y[i]),
            .s   (sum[i]),
            .cin (carry[i]),
            .cout(carry[i+1])
        );
    end

    assign cout = carry[WIDTH];

endmodule

// register, a register
module register #(
    parameter int WIDTH
) (
    input  logic             clk,
    input  logic             rst,
    input  logic             en,
    input  logic [WIDTH-1:0] in,
    output logic [WIDTH-1:0] out
);
    always_ff @(posedge clk or posedge rst) begin
        if (rst) out <= '0;
        else if (en) out <= in;
    end
endmodule

module delay #(
    parameter int CYCLES = 4,
    parameter int WIDTH  = 8
) (
    input  logic             clk,
    input  logic             rst,
    input  logic             en,
    input  logic [WIDTH-1:0] in,
    output logic [WIDTH-1:0] out
);
    // Ideally, every module would validate its parameters because often
    // certain values are undefined. For example, a negative cycles delay
    // doesn't make sense. Similarly, only positive widths make sense.
    // Unfortunately, parameter validation is somewhat lacking in SystemVerilog,
    // at least in the older versions. Some of the possibilities are shown
    // below.

    // My preferred method of parameter validation is the following, which seems
    // to be supported by most tools. I would recommend trying these first, and then 
    // resorting to the following alternatives.
    initial begin
        if (CYCLES < 0) $fatal(1, "ERROR: Cycles must be >= 0.");
        if (WIDTH < 1) $fatal(1, "ERROR: Width must be positive.");
    end

    // Assertions seem like a natural choice for validation, but Quartus ignores
    // these:
    /*
    always_comb assert(CYCLES >= 0);
    assert property (@(posedge clk) CYCLES >= 0);
    */

    // Alternatively, we can use if generates for validation.
    if (CYCLES < 0) begin
        // One workaround to missing validation constructs is to simply call
        // an undefined module with a name that specifies the error.
        cycles_parameter_must_be_ge_0();

        // The 2009 SV standard defines the $error function, which prints during
        // compilation. However, not every tool supports it yet. Also, despite 
        // the name, $error only created a warning in the version of Quartus
        // used for testing.
        //
        //$error("ERROR: CYCLES parameter must be >= 0.");

        // $fatal does cause Quartus synthesis to terminate, but is not 
        // supported by every synthesis tool.
        //
        //$fatal(1, "ERROR: CYCLES parameter must be >= 0.");
    end
    if (WIDTH < 1) begin
        width_parameter_must_be_gt_0();
        //$error("ERROR: WIDTH parameter must be >= 1.");
        //$fatal(1, "ERROR: WIDTH parameter must be >= 1.");      
    end

    // Create an array of WIDTH-bit signals, which will connect all the registers
    // together (see delay.pdf). The array uses CYCLES+1 elements because there
    // are CYCLES register outputs, plus the input to the first register.
    //
    // When creating an array this way, the CYCLES+1 section creates an unpacked
    // array. The [WIDTH-1:0] section creates a packed array. Packed arrays and
    // unpacked arrays support different operations, but generally you will use
    // the packed section to specify bits, and the unpacked section to specify
    // the total number of elements.
    //
    // The CYCLES+1 notation is short for [0:CYCLES+1-1]. A common convention is
    // to use "downto" syntax for the packed array, and "to" syntax for the
    // unpacked array. Most people are used to thinking of arrays starting at 
    // index 0, and the MSB starting at the highest number.
    //
    // VHDL COMPARISON: packed arrays are missing from VHDL, where you instead 
    // have to create a custom array type. In SV, every signal can become an
    // unpacked array simply by adding [], which is very convenient.
    logic [WIDTH-1:0] regs[CYCLES+1];

    if (CYCLES == 0) begin : l_cycles_eq_0
        // For CYCLES == 0, there is no delay, so just use a wire.
        assign out = in;
    end else if (CYCLES > 0) begin : l_cycles_gt_0
        // Create a sequence of CYCLES registers, where each register adds one
        // cycle to the delay.

        // NOTE: Old versions of SV and some tools (e.g., Quartus_ require the 
        // genvar to be declared outside the loop. 
        for (genvar i = 0; i < CYCLES; i++) begin : reg_array
            register #(
                .WIDTH(WIDTH)
            ) reg_array (
                // If signals share the same name as the I/O, the parentheses can
                // be ommitted. I don't recommend this style because it can be
                // very error prone when you change the name of either the I/O or the
                // signal.
                .clk,
                .rst,
                .en,
                .in (regs[i]),
                .out(regs[i+1])
            );
        end

        // The first register's input comes from the delay's input.
        assign regs[0] = in;

        // The last register's output goes to the delay's output.
        assign out = regs[CYCLES];
    end
endmodule
```

# Registers
```systemverilog
module register_async_rst#(
	parameter int WIDTH
)(
	input logic clk,
	input logic rst,
	input logic [WIDTH-1:0] din,
	output logic [WIDTH-1:0] dout
);

	always_ff @(posedge clk or posedge rst) begin
		if (rst) begin
			dout <= '0; // use non-blocking assignments
		end
		else begin
		// Synthesis rule, any non-blocking assignment to a signal on a rising clock edge will be synthesized as a register.
		// Make sure to use a non-blocking assignment
			dout <= din;
		end
	end
endmodule


module register_async_rst2#(
	parameter int WIDTH
)(
	input logic clk,
	input logic rst,
	input logic [WIDTH-1:0] din,
	output logic [WIDTH-1:0] dout
);
	// same thing, but can synthesize less hardware. read stitt article about this
	always_ff @(posedge clk or posedge rst) begin
		dout <= din;
		if (rst) dout <= '0;
	end
endmodule

// older fpga --> async reset
// newer fpga --> sync reset
module register_sync_rst#(
	parameter int WIDTH
)(
	input logic clk,
	input logic rst,
	input logic [WIDTH-1:0] din,
	output logic [WIDTH-1:0] dout
);

	always_ff @(posedge clk) begin
		dout <= din;
		if (rst) out <= '0;
	end
endmodule

register_en_async_rst#(
	parameter int WIDTH
)(
	input logic clk,
	input logic rst,
	input logic en, 
	input logic [WIDTH-1:0] din,
	output logic [WIDTH-1:0] dout
);
	always_ff @(posedge clk or posedge rst) begin
		if (en) out <= in;
		if (rst) out <= '0;
	end
endmodule

register_en_sync_rst#(
	parameter int WIDTH
)(
	input logic clk,
	input logic rst,
	input logic en, 
	input logic [WIDTH-1:0] din,
	output logic [WIDTH-1:0] dout
);
	always_ff @(posedge clk) begin
		if (en) out <= in;
		if (rst) out <= '0;
	end
endmodule
```
# Sequential Architectures
```systemverilog
module arch1 #(
    parameter int WIDTH
) (
    input  logic             clk,
    input  logic             rst,
    input  logic [WIDTH-1:0] in1,
    input  logic [WIDTH-1:0] in2,
    input  logic [WIDTH-1:0] in3,
    output logic [WIDTH-1:0] out1,
    output logic [WIDTH-1:0] out2
);
	
	always_ff @(posedge clk) begin
		// logic [WIDTH-1:0] out1_r, out2_r;
		out1 <= in1 + in2; // ignore top bit
		out2 <= in3; 
		if (rst) begin
			out1 <= '0;
			out2 <= '0;
		end
	end
endmodule



module arch2 #(
    parameter int WIDTH
) (
    input  logic             clk,
    input  logic             rst,
    input  logic [WIDTH-1:0] in1,
    input  logic [WIDTH-1:0] in2,
    input  logic [WIDTH-1:0] in3,
    output logic [WIDTH-1:0] out1,
    output logic [WIDTH-1:0] out2
);
	logic [WIDTH-1:0] in1_r, in2_r;
	always_ff @(posedge clk) begin
		in1_r <= in1;
		in2_r <= in2;
		out1 <= in1_r + in2_r; // ignore top bit
		out2 <= in3; 
		if (rst) begin
			in1_r <= '0;
			in2_r <= '0;
			out1 <= '0;
			out2 <= '0;
		end
	end
endmodule


module arch3 #(
    parameter int WIDTH
) (
    input  logic             clk,
    input  logic             rst,
    input  logic [WIDTH-1:0] in1,
    input  logic [WIDTH-1:0] in2,
    input  logic [WIDTH-1:0] in3,
    output logic [WIDTH-1:0] out1,
    output logic [WIDTH-1:0] out2
);
	logic [WIDTH-1:0] in1_r, in2_r, in3_r, add_out1_r, add_out2_r;
	always_ff @(posedge clk) begin
		in1_r <= in1;
		in2_r <= in2;
		in3_r <= in3;
		add_out1_r <= in1_r + in2_r;
		// you can remove add_out1_r to just be out1 and then read out1 in add_out2_r as out1 has now been registered. this is extremely useful, and different from systemverilog. 
		add_out2_r <= add_out1_r; 
		if (rst) begin
			in1_r <= '0;
			in2_r <= '0;
			in3_r <= '0;
			add_out1_r <= '0;
			add_out2_r <= '0;
		end
	end
	assign out1 = add_out1_r;
	assign out2 = in3_r + add_out2_r;
endmodule

// bad way to do this
module arch4_warnings #(
    parameter int WIDTH
) (
    input  logic             clk,
    input  logic             rst,
    input  logic [WIDTH-1:0] in1,
    input  logic [WIDTH-1:0] in2,
    input  logic [WIDTH-1:0] in3,
    output logic [WIDTH-1:0] out1,
    output logic [WIDTH-1:0] out2
);
	logic [WIDTH-1:0] in1_r, in2_r, in3_r, /*add_out*/;
	// warning comes from using blocking assignment comb logic inside an always_ff. if it were just an always, it'd be fine to fix and no warnings.
	// if we use a blocking assignment on a signal inside an always-ff or an always block tied to the clock, we can introduce many issues like inferring more registers, introducing race conditions, and potentially undefined behavior. either don't do it, or if you do it, limit the scope of the blocking assigned signal to only the clocked block.
	always_ff @(posedge clk) begin
		logic [WIDTH-1:0] add_out; // now add_out won't potentially cause issues outside of the block, since the scope is limited to the block. 
		// If a signal assigned with a blocking assignmnet is always defined before being read, then it will be a wire
		add_out = in1_r + in2_r;
		{out1, out2} <= add_out * in3_r;
		in1_r <= in1;
		in2_r <= in2;
		in3_r <= in3;
		if (rst) begin
			in1_r <= '0;
			in2_r <= '0;
			in3_r <= '0;
			out1 <= '0;
			out2 <= '0;
		end
	end
endmodule


module arch4 #(
    parameter int WIDTH
) (
    input  logic             clk,
    input  logic             rst,
    input  logic [WIDTH-1:0] in1,
    input  logic [WIDTH-1:0] in2,
    input  logic [WIDTH-1:0] in3,
    output logic [WIDTH-1:0] out1,
    output logic [WIDTH-1:0] out2
);
	logic [WIDTH-1:0] in1_r, in2_r, in3_r, add_out;

	always_ff @(posedge clk) begin
		{out1, out2} <= add_out * in3_r;
		in1_r <= in1;
		in2_r <= in2;
		in3_r <= in3;
		if (rst) begin
			in1_r <= '0;
			in2_r <= '0;
			in3_r <= '0;
			out1 <= '0;
			out2 <= '0;
		end
	end
		
	// take the one thing without a registered output and put it in the combinational block. 
	always_comb begin
		add_out = in1_r + in2_r; 
	end
endmodule



module arch5 #(
    parameter int WIDTH
) (
    input  logic             clk,
    input  logic             rst,
    input  logic [WIDTH-1:0] in1,
    input  logic [WIDTH-1:0] in2,
    input  logic [WIDTH-1:0] in3,
    output logic [WIDTH-1:0] out1,
    output logic [WIDTH-1:0] out2
);
	logic [WIDTH-1:0] in1_r, in2_r, in3_r, add_out, mult_out;

	always_ff @(posedge clk) begin
		out1 <= add_out;
		out2 <= mult_out;
		in1_r <= in1;
		in2_r <= in2;
		in3_r <= in3;
		if (rst) begin
			in1_r <= '0;
			in2_r <= '0;
			in3_r <= '0;
			out1 <= '0;
			out2 <= '0;
		end
	end
		
	// take the one thing without a registered output and put it in the combinational block. 
	always_comb begin
		add_out = in1_r + in2_r; 
		mult_out = add_out * in3_r;
	end
endmodule


module arch6 #(
    parameter int WIDTH
) (
    input  logic             clk,
    input  logic             rst,
    input  logic [WIDTH-1:0] in1,
    input  logic [WIDTH-1:0] in2,
    input  logic [WIDTH-1:0] in3,
    output logic [WIDTH-1:0] out1,
    output logic [WIDTH-1:0] out2
);
	logic [WIDTH-1:0] in1_r, in2_r, in3_r;

	always_ff @(posedge clk) begin
		in1_r <= in1;
		in2_r <= in2;
		in3_r <= in3;
		// out1 needs to be read again and value needs to be preserved, so a register is synthesized here. we can assign it outside combinationally and read it in out2 and it'll synthesize to the behavior we want
		// out1 = in1_r + in2_r;
		out2 <= out1 * in3_r;
		if (rst) begin
			in1_r <= '0;
			in2_r <= '0;
			in3_r <= '0;
			out1 <= '0;
			out2 <= '0;
		end
	end
	
	assign out1 = in1_r + in2_r
endmodule
```

# FSMs - Moore&Mealy - 1/2/2+ Processes
```systemverilog
module moore_1p (
    input  logic       clk,
    input  logic       rst,
    input  logic       en,
    output logic [3:0] out
);
typedef enum logic [1:0] {
	STATE0,
	STATE1,
	STATE2,
	STATE3
} state_t;

state_t state_r;

always_ff @(posedge clk) begin

	case (state_r) 
		STATE0: begin
			out <= 4'b0001;
			if (en) state_r <= STATE1;
			// don't need the else because state_r is already a register and will preserve the value automatically
			// else state_r <= STATE0;
		end
		
		STATE1: begin
			out <= 4'b0010;
			if (en) state_r <= STATE2;
			// else state_r <= STATE1;
		end
		
		STATE2: begin
			out <= 4'b0100;
			if (en) state_r <= STATE3;
			// else state_r <= STATE2;
		end
		
		STATE3: begin
			out <= 4'b1000;
			if (en) state_r <= STATE0;
			// else state_r <= STATE3;
		end
	endcase
	
	if (rst) begin
		state_r <= STATE0;
		out <= 4'b0001;
	end
end

// here, since we assigned out <= 4'bXXXX, we accidentally created registered outputs! this will mess up with all our timings.
// 1 process is buns
endmodule

module moore_2p begin(
    input  logic       clk,
    input  logic       rst,
    input  logic       en,
    output logic [3:0] out
);
	typedef enum logic [1:0] {
		STATE0,
		STATE1,
		STATE2,
		STATE3
	} state_t;
);

	state_t state_r, next_state;
	// async reset
	always_ff @(posedge clk or posedge rst) begin
		state_r <= next_state;
		if (rst) state_r <= next_state;
	end
	
	always_comb begin
		case (state_r) 
			STATE0: begin
				out = 4'b0001;
				if (en) next_state = STATE1;
				else next_state = STATE0;
			end
			
			STATE1: begin
				out = 4'b0010;
				if (en) next_state = STATE2;
				else state_r = STATE1;
			end
			
			STATE2: begin
				out = 4'b0100;
				if (en) next_state = STATE3;
				else state_r = STATE2;
			end
			
			STATE3: begin
				out = 4'b1000;
				if (en) next_state = STATE0;
				else state_r = STATE3;
			end
			
			default: begin
			end
	endcase
	end
endmodule

// this can save a little bit of code for the same behavior
module moore_2p_2 begin(
    input  logic       clk,
    input  logic       rst,
    input  logic       en,
    output logic [3:0] out
);
	typedef enum logic [1:0] {
		STATE0,
		STATE1,
		STATE2,
		STATE3
	} state_t;
);

	state_t state_r, next_state;
	// async reset
	always_ff @(posedge clk or posedge rst) begin
		state_r <= next_state;
		if (rst) state_r <= next_state;
	end
	
	always_comb begin
		next_state = state_r; // now we don't need else
		case (state_r) 
			STATE0: begin
				out = 4'b0001;
				if (en) next_state = STATE1;
				//else next_state = STATE0;
			end
			
			STATE1: begin
				out = 4'b0010;
				if (en) next_state = STATE2;
				//else state_r = STATE1;
			end
			
			STATE2: begin
				out = 4'b0100;
				if (en) next_state = STATE3;
				//else state_r = STATE2;
			end
			
			STATE3: begin
				out = 4'b1000;
				if (en) next_state = STATE0;
				//else state_r = STATE3;
			end
			
			default: begin
			end
	endcase
	end
endmodule


module mealy_2p (
    input  logic clk,
    input  logic rst,
    input  logic go,
    input  logic ack,
    output logic en,
    output logic done
);

    typedef enum logic [1:0] {
        START,
        COMPUTE,
        FINISH,
        RESTART
    } state_t;
    state_t state_r, next_state;

    always_ff @(posedge clk or posedge rst) begin
        state_r <= next_state;
        if (rst) state_r <= START;
    end

    always_comb begin
        case (state_r)
            START: begin
                // In a Mealy FSM, outputs are a function of the state and the
                // current input, which makes them associated with transitions.
                // Therefore, all outputs appear within if statements that
                // correspond to transitions between states.
                if (go) begin
                    done       = 1'b0;
                    en         = 1'b0;
                    next_state = COMPUTE;
                end else begin
                    done       = 1'b0;
                    en         = 1'b0;
                    next_state = START;
                end
            end

            COMPUTE: begin
                if (ack) begin
                    en         = 1'b0;
                    done       = 1'b1;
                    next_state = FINISH;
                end else begin
                    en         = 1'b1;
                    done       = 1'b0;
                    next_state = COMPUTE;
                end
            end

            FINISH: begin
                if (go) begin
                    done       = 1'b1;
                    en         = 1'b0;
                    next_state = FINISH;
                end else begin
                    done       = 1'b1;
                    en         = 1'b0;
                    next_state = RESTART;
                end
            end

            RESTART: begin
                if (go) begin
                    done       = 1'b0;
                    en         = 1'b0;
                    next_state = COMPUTE;
                end else begin
                    done       = 1'b1;
                    en         = 1'b0;
                    next_state = RESTART;
                end
            end
        endcase
    end
endmodule  // mealy
```

# Testbenches & Gotchas
- Monolithic Testbench - kinda this
	- Better for simple testbenches
	- More complex testbenches aren't monolithic
```systemverilog
`timescale 1 ns / 100 ps // need this

module mux2x1_tb;
// no parameters (yet)

	logic in1, in1, sel, out;
	mux2x1 DUT (
		.in0(in0),
		.in1(in1),
		.sel(sel),
		.out(out),
	)
	
	
	// think of this as an always block, but it only runs once (unless specified otherwise)
	// this will run at the beginning of the simulation
	logic expected_out;
	initial begin
		$time format(-9, 0, " ns"); // necessary for printing time with $display(). gripe with sv
		in0 <= 1'b0;
		in1 <= 1'b1;
		sel <= 1'b1;
		#10; // wait 10 time steps (10ns for our timescale)
		
		// assert correct value and print error elsewhere
		expected_out = sel ? in1 : in0;
		if (out != expected_out) begin
			$error("out = %b instead of %b", $time, out, expected_out);
		end
	end
endmodule
// This testbench sucks. Lets make a tb that covers every possibility of inputs

module mux2x1_tb1;
	logic in1, in1, sel, out;
	mux2x1 DUT (
		.in0(in0),
		.in1(in1),
		.sel(sel),
		.out(out),
	)
	
	initial begin
		for (int i = 0; i < 8; i++) begin
			in0 <= i[0];
			in1 <= i[1];
			sel <= i[2]; 
			// `i` is a 32-bit integer, so we're indexing the bits to drive
			// our testbench.
			// int --> expected_out 
			// 000 --> 0
			// 001 --> 1
			// 010 --> 0
			// ...
			expected_out = sel ? in1 : in0;
		end
		// assert correct value and print error elsewhere
		// should always be true
		assert(out == expected_out) else $error("out = %b instead of %b", $time, out, expected_out); // print error message if false 
	end

endmodule


module mux2x1_tb2;
	logic in1, in1, sel, out;
	mux2x1 DUT (
		.in0(in0),
		.in1(in1),
		.sel(sel),
		.out(out),
	)
	
	// generate a clock signal w/ 10ns period.
	always begin
		forever #5 clk <= !clk; // forever here is redundant, but not bad to use
	end
	
	initial begin
		for (int i = 0; i < 8; i++) begin
			in0 <= i[0];
			in1 <= i[1];
			sel <= i[2]; 
			@(posedge clk); // wait until clock is high again!
			// used all the time, but is annoying in simulation.
			expected_out = sel ? in1 : in0; // assigned after clock edge. in simulation, the waveform for expected_out is delayed compared to the out signal. if we tried to move it before the clock edge, we get a bunch of errors because of NBA!! Because NBA, the signals hadn't been driven yet so no update.
			// However, if you wanted to keep it before the clock edge, you might try to fix it using blocking assignments, bad! Could introduce race conditions on the code
		end
		assert(out == expected_out) else $error("out = %b instead of %b", $time, out, expected_out); // print error message if false 
		$display("Tests completed.");
		
		
	end

endmodule

module mux2x1_tb3;
	logic in1, in1, sel, out;
	mux2x1 DUT (
		.in0(in0),
		.in1(in1),
		.sel(sel),
		.out(out),
	)
	
	// generate a clock signal w/ 10ns period. : INITAL INSTEAD OF ALWAYS
	initial begin : generate_clock
		forever #5 clk <= !clk;
	end
	
	initial begin
		for (int i = 0; i < 8; i++) begin
			in0 <= i[0];
			in1 <= i[1];
			sel <= i[2]; 
			@(posedge clk); 
			expected_out = sel ? in1 : in0; 
		end
		assert(out == expected_out) else $error("out = %b instead of %b", $time, out, expected_out); // print error message if false 
		$display("Tests completed.");
		$finish; // makes the tb end "run -all" BUT if you click yes on the popup it will close questa! Better for command line, and that's annoying so use this instead
		disable generate_clock; 
		// now, if we "run -all" it just runs and doesn't give you that popup and it's better in the gui format. 
		
	end
endmodule
```
## Registers TB
```systemverilog
module registers_tb2;
	localparam NUM_TESTS = 10000;
	localparam WIDTH = 8;
	logic clk <= 1'b0, rst, en, prev_en; // always initialize clock
	logic [WIDTH-1:0] in, out;
	logic [WIDTH-1:0] prev_in, prev_out;
	
	register #(.WIDTH(WIDTH)) DUT (.*); // glob all the same name and tie to dut

	initial begin : gen_clock
		forever #5 clk <= !clk;
	end
	
	initial begin : provide_stim
		rst <= 1'b1;
		in <= '0;
		en <= 1'b1;
		repeat (5) @(posedge clk);
		@(negedge clk);
		rst <= 1'b0;
		repeat (2) @(posedge clk);
		
		for (inti = 0; i < NUM_TESTS; i++) begin
			in <= $urandom;
			en <= 1'b1;
			@(posedge clk);
		end
		disable gen_clk;
	end
	
	initial begin : check_outputs
		forever begin
			$time format(-9, 0, " ns");
			@(posedge clk);
			prev_in = in; // also bad, but since it's not used anywhere else it won't introduce any race conditions.
			prev_en = en;
			prev_out - out;
			@(negedge clk); // not good, but can be used so condition sampled at the right time and properly. 
			// if out !- in error!!
			if (out != in) $error("[%0t]Expected=%d, Actual=%d", in, out);
			if (prev_en && prev_in != out) $error("[%0t]Expected=%d, Actual=%d", in, out);
			if (!prev_en && out != prev_out) $error("[%0t]Expected=%d, Actual=%d", in, out);)
endmodule
// this top one is basically garbage. it works, but it's so complicated for no reason

module registers_tb3;
	localparam NUM_TESTS = 10000;
	localparam WIDTH = 8;
	logic clk <= 1'b0, rst, en, prev_en; // always initialize clock
	logic [WIDTH-1:0] in, out;
	logic [WIDTH-1:0] prev_in, prev_out;
	
	register #(.WIDTH(WIDTH)) DUT (.*); // glob all the same name and tie to dut

	initial begin : gen_clock
		forever #5 clk <= !clk;
	end
	
	initial begin : provide_stim
		rst <= 1'b1;
		in <= '0;
		en <= 1'b1;
		repeat (5) @(posedge clk);
		@(negedge clk);
		rst <= 1'b0;
		repeat (2) @(posedge clk);
		
		for (inti = 0; i < NUM_TESTS; i++) begin
			in <= $urandom;
			en <= 1'b1;
			// place previouses here so we can start the test earlier and not have to wait. it works because we no longer have to check for the current out, but instead we're checking only the previous output. 
			prev_in <= in; 
			prev_en <= en;
			prev_out <= out;
			@(posedge clk);
		end
		disable gen_clk;
	end
	
	initial begin : check_outputs
		forever begin
			$time format(-9, 0, " ns");
			@(posedge clk);
			if (out != in) $error("[%0t]Expected=%d, Actual=%d", in, out);
			if (prev_en && prev_in != out) $error("[%0t]Expected=%d, Actual=%d", in, out);
			if (!prev_en && out != prev_out) $error("[%0t]Expected=%d, Actual=%d", in, out);)
endmodule
// this is slightly better, but still in no way good, especialyl if it's this complex still for just a register. 

module registers_tb4;
	localparam NUM_TESTS = 10000;
	localparam WIDTH = 8;
	logic clk <= 1'b0, rst, en, ; // always initialize clock
	logic [WIDTH-1:0] in, out, expected;
	// get rid of "previous" stuff
	register #(.WIDTH(WIDTH)) DUT (.*); // glob all the same name and tie to dut

// let's decompose the tb into finer and finer responsibilities
	initial begin : gen_clock
		forever #5 clk <= !clk;
	end
	
	initial begin : provide_stim
		rst <= 1'b1;
		in <= '0;
		en <= 1'b1;
		repeat (5) @(posedge clk);
		@(negedge clk);
		rst <= 1'b0;
		repeat (2) @(posedge clk);
		
		for (inti = 0; i < NUM_TESTS; i++) begin
			in <= $urandom;
			en <= 1'b1;
			@(posedge clk);
		end
		disable gen_clk;
	end
	
	initial begin : monitor
		// essentially will check for some condition where there is a valid output 
		@(posedge clk iff !rst);
		forever begin
			expected <= en ? in : out; // if enabled, expected output should be the input of the DUT, else it's the output
			@(posedge clk); 
		end
	end
	
	initial begin : check_outputs
		@(posedge clk iff !rst); // if and only if not resetted
		forever begin
			$time format(-9, 0, " ns");
			@(posedge clk); 
			if (expected != out) $error("Expected = %0h, Actual = %0h", exoected, out);
			if (!prev_en !== 1'b1 && prev_out !== out)	$error("Expected = %0h, Actual = %0h", exoected, out);
		end
		// use !== to check for undefined values (like testing during reset)
		// ditto for ==, use ===
endmodule
```
# Assertions (SVAs)
```systemverilog
// instant assertion
module registers_tb4;
	localparam NUM_TESTS = 10000;
	localparam WIDTH = 8;
	logic clk <= 1'b0, rst, en, ; // always initialize clock
	logic [WIDTH-1:0] in, out, expected;
	// get rid of "previous" stuff
	register #(.WIDTH(WIDTH)) DUT (.*); // glob all the same name and tie to dut

// let's decompose the tb into finer and finer responsibilities
	initial begin : gen_clock
		forever #5 clk <= !clk;
	end
	
	initial begin : provide_stim
		rst <= 1'b1;
		in <= '0;
		en <= 1'b1;
		repeat (5) @(posedge clk);
		@(negedge clk);
		rst <= 1'b0;
		repeat (2) @(posedge clk);
		
		for (inti = 0; i < NUM_TESTS; i++) begin
			in <= $urandom;
			en <= 1'b1;
			@(posedge clk);
		end
		disable gen_clk;
	end
	
	initial begin : monitor
		// essentially will check for some condition where there is a valid output 
		@(posedge clk iff !rst);
		forever begin
			expected <= en ? in : out; // if enabled, expected output should be the input of the DUT, else it's the output
			@(posedge clk); 
		end
	end
	
	initial begin : check_outputs
		@(posedge clk iff !rst); // if and only if not resetted
		forever begin
			$time format(-9, 0, " ns");
			@(posedge clk); 
			// !!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!
			// achieves the same thing as the if statement
			assert(expected == out) else $error("Expected = %0h, Actual = %0h", expected, out);	// with the else we can print the custom message. immediate assertions don't really save any space in the code, but it's just an example of the simplest assertion you can have.
			// immediate assertion must occur inside a block where we have sequential code 
		end
endmodule


module ff_tb#(
	parameter int NUM_TESTS = 1000;
);
	logic clk = 1'b0, rst, en, in, out;
	
	ff DUT (
		.en(1'b1);
		.*
	)
	
	
	initial begin : clk_gen
		forever #5 clk = ~clk;
	end
	
	initial begin : stim_proc
		$timeformat(-9, 0, " ns");
		rst <= 1'b1;
		in <= 1'b0;
		repeat (5) @(posedge clk);
		@(negedge clk);
		rst <= 1'b0;
		
		for (int i = 0; i < NUM_TESTS; i++) begin
			in <= $urandom;
			@(posedge clk);
		end
		
		disable clk_gen;
		$display("Test Completed.");
	end
	
	// beauty of assertions
	// sequences - construct that allows us to create boolean expressions over an amount of assertions in a range of time
	// a ##2 b ##2 c ##5 !d; this is a boolean expression that captures a range of time
	// concurrent assertion. sometimes called a property
	// ( [time to sample] [ assertion to make ])
	// sequences by themselves aren't useful, but become more useful when they're combined with **implications**
	
	
	// this is something someone would write to emulate the property of a flipflop (not worrying about the enable) but this is bad for a reason
	
	// assert property (@(posedge clk) in ##1 out);
	
	// what this is actually saying is "for every clock cycle, in should be 1 and one clock cycle later out should be 1"
	// we certainly don't want this, as sometimes in can be 0
	
	// to fix this, lets create an implication (|->)
	
	// assert property (@(posedge clk) in |-> ##1 out);
	
	// this operator is called the implication operator. the value preceding the operator is called the antecedent or trigger condition, the value on the right is the consequence
	// this is saying, if the condition on the left is false, the entire expression becomes "vacuously true", so the implication only becomes applied when the trigger condition is asserted. hence, when in=0, this won't trigger the implication and not assume in should always be asserted
	// now it's saying if in=1, on the next clock cycle out should be 1
	
	// now, that assertion isn't perfect, as it doesn't account for the reset, but if we ran this testbench as is, our input is 0 during the reset phase so the property never triggers and never prints an error. let's make this account for the reset using a disable statement
	
	// now this says, if rst is asserted, we don't want to evaluate this. with this property, we have a very good replacement of our monitor and output_check from the previous register testbenches. we can eventually capture an enormous amount of functionaltity with these assertion properties
	assert property (@(posedge clk) disable iff (rst) in |-> ##1 out);


endmodule


module ff_tb#(
	parameter int NUM_TESTS = 1000;
);
	logic clk = 1'b0, rst, en, in, out;
	
	ff DUT (
		.en(1'b1);
		.*
	)
	
	
	initial begin : clk_gen
		forever #5 clk = ~clk;
	end
	
	initial begin : stim_proc
		$timeformat(-9, 0, " ns");
		rst <= 1'b1;
		in <= 1'b0;
		repeat (5) @(posedge clk);
		@(negedge clk);
		rst <= 1'b0;
		
		for (int i = 0; i < NUM_TESTS; i++) begin
			in <= $urandom;
			@(posedge clk);
		end
		
		disable clk_gen;
		$display("Test Completed.");
	end
	
	
	assert property (@(posedge clk) disable iff (rst) in |-> ##1 out);
	 // |=> is shorthand for  |-> ##1
	assert propert (@(posedge clk) disable iff (rst) in |=> out);
	
	assert property (@(posedge clk) rst |=> !out); 
    always @(posedge rst) #1 assert (out == 1'b0);
    // here we can test for when the reset is enabled that 1 clock cycle later, out is 0. Top is for synchronous reset, bottom is for asynchronous reset


endmodule

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
    assert property (@(posedge clk) disable iff (rst) out == $past(in, 1, en)); 
    // might work, we can use $past to get previous values without having to define them and sample them ourselved. i think we can pass in the enable parameter
    // however, this doesn't test the entire system, as we don't test for when the enable isn't asserted
    assert property(@(posedge clk) dsiable iff (rst) !en |=> out == $past(out,1));
    assert property(@(posedge clk) dsiable iff (rst) !en |=> $stable(out));
    // both of these do the same thing where it makes sure the value of out doesn't change if the the enable isn't asserted. 
    assert property (@(posedge clk) rst |=> !out); 
    always @(posedge rst) #1 assert (out == 1'b0);

endmodule;   

module delay_tb1#(
	parameter int NUM_TESTS = 1000,
	parameter int CYCLES = 4,
	parameter int WIDTH = 8
);
	
	logic clk = 1'b0, rst, en;
	logic [WIDTH-1:0] data_in, data_out;
	
	delay #(
		.CYCLES(CYCLES),
		.WIDTH(WIDTH)
	) DUT (
		.*
	);
	
	initial begin : clk_gen
		forever #5 clk <= ~clk;
	end
	
	initial begin : stim_proc
		$timeformat(=9, 0, " ns");
		
		rst <= 1'b1;
		en <= 1'b0;
		data_in <= '0;
		repeat (5) @(posedge clk);
		@(negedge clk);
		rst <= 1'b0;
		
		for (int i = 0; i < NUM_TESTS; i++) begin
			data_in <= $urandom;
			en <= $urandom;
			@(posedge clk);
		end
		
		disable clk_gen;
		disable check_output;
		$display("Tests completed.");
	end
	
	// here we can create a half decent delay model to verify the DUT
	// round up the buffer to the next power of 2, which is necessary because of addressing logic
	localparam int BUFFER_SIZE = 2 ** $clog2(CYCLES);
	// now reset the contents
	logic [WIDTH-1:0] buffer[BUFFER_SIZE] = '{default: '0};
	// addresses for acessing the buffer. inputs are written to buffer and outputs are read from buffer
	logic [$clog2(CYCLES)-1:0] wr_addr = 0;
	logic [$clog2(CYCLES)-1:0] rd_addr;
	
	logic [WIDTH-1:0] correct_out;
	// this read address should be offset from the write addres by CYCLES+1
	assign rd_addr = wr_addr - CYCLES + 1;
	
	// generate reference model
	generate
		if (CYCLES==0) begin
			assign correct_out = data_in;
		end else begin
			always @(posedge clk or posedge rst) begin
				if (rst) correct_out <= '0;
				else if (en) begin
					buffer[wr_addr] = data_in;
					correct_out <= buffer[rd_addr];
					wr_addr <= wr_addr + 1'b1;
				end
			end
		end
	endgenerate
	
	initial begin : check_output
		forever begin
			@(posedge clk);
			if (data_out != correct_out) $error("[%0t] out = %0h instead of %0h.", $realtime, data_out, correct_out);
		end
	end
	
	// this assert property can replace check_output if we wanted
	assert property (@(posedge clk) disable iff (rst) data_out == correct_out);

	// issue here is that the generated model is more complex than the DUT itself, even though the DUT is simple, it's still annoying.
	// lets do something else
endmodule

module delay_tb2#(
	parameter int NUM_TESTS = 1000,
	parameter int CYCLES = 4,
	parameter int WIDTH = 8
);
	
	logic clk = 1'b0, rst, en;
	logic [WIDTH-1:0] data_in, data_out;
	
	delay #(
		.CYCLES(CYCLES),
		.WIDTH(WIDTH)
	) DUT (
		.*
	);
	
	initial begin : clk_gen
		forever #5 clk <= ~clk;
	end
	
		initial begin : stim_proc
		$timeformat(=9, 0, " ns");
		
		rst <= 1'b1;
		en <= 1'b0;
		data_in <= '0;
		repeat (5) @(posedge clk);
		@(negedge clk);
		rst <= 1'b0;
		
		for (int i = 0; i < NUM_TESTS; i++) begin
			data_in <= $urandom;
			en <= $urandom;
			@(posedge clk);
		end
		
		disable clk_gen;
		disable check_output;
		$display("Tests completed.");
	end
	
	// lets use another construct that's in systemverilog we can take advantage of
	// a queue!
	logic [WIDTH-1:0] model_queue[$];
	
	// if we think about it, a delay can be modeled as a queue, where the number of delay cycles matches the amount of times we push into the queue before we take our first read
	
	// lets generate the model
	always ff @(posedge clk or posedge rst) begin
		// on reset, initialize the queue with CYCLES 0 values to mimic the reset behaviour of the DUT
		if (rst) begin
			model_queue = {}; // reset it
			for (int i = 0; i < CYCLES; i++) model_queue.push_back('0); // fill it
		end else if (en) begin
			// update the queue by popping the front and pushing the new input
			// note that these are blocking assignments
			
			// the void cast is not necessary, but if we call a function that returns a value and don't assign anything, the simulator will report a warning, like if we did a [[nodiscard]] in c++. casting to void suppresses that error
			void'(model_queue.pop_front());
			model_queue.push_back(data_in);
			// alternatively, we can use this syntax but this is pretty unreadable
			//model_queue = {model_queue[1:$], data_in};
		end
	end
	
	// simpler model than previous one, so now we can bring back the assertion and use this. however, there's another much simpler way to do this
	assert property (@(posedge clk) disable iff (rst) data_out == correct_out);
endmodule

module delay_tb3#(
	parameter int NUM_TESTS = 1000,
	parameter int CYCLES = 4,
	parameter int WIDTH = 8
);
	
	logic clk = 1'b0, rst, en;
	logic [WIDTH-1:0] data_in, data_out;
	
	delay #(
		.CYCLES(CYCLES),
		.WIDTH(WIDTH)
	) DUT (
		.*
	);
	
	initial begin : clk_gen
		forever #5 clk <= ~clk;
	end
	
		initial begin : stim_proc
		$timeformat(=9, 0, " ns");
		
		rst <= 1'b1;
		en <= 1'b0;
		data_in <= '0;
		repeat (5) @(posedge clk);
		@(negedge clk);
		rst <= 1'b0;
		
		for (int i = 0; i < NUM_TESTS; i++) begin
			data_in <= $urandom;
			en <= $urandom;
			@(posedge clk);
		end
		
		disable clk_gen;
		disable check_output;
		$display("Tests completed.");
	end
	
	// our goal here is to try to do it solely with assertions
	// assert property (@(posedge clk) disable iff (rst) data_out == $past(data_in, CYCLES, en));
	// same problem here, until there have been CYCLES pushes into the delay, this assertion will fail the early testbench. 
	
	// lets try to fix this with a counter
	int count;
	always_ff @(posedge clk or posedge rst) begin
		if (rst) count <= 0;
		else if (en && count < CYCLES) count <= count + 1;
	end
	
	assert property (@(posedge clk) disable iff (rst) count == CYCLES |-> data_out == $past(data_in, CYCLES, en)); 
	// now this works, but we're leaving out CYCLES number of cycles where the TB isn't testing your dut. all testbenches should be testing everyting at every clock cycle for functionaltiy
	assert propert (@(posedge clk) disable iff (rst) count < CYCLES |-> data_out == '0);
	// now we're testing for the remaining CYCLES number of cycles
	assert property (@(posedge clk) disable iff (rst) !en |-> $stable(data_out));
	// now we're testing for if the enable isn't asserted that the delay shouldn't move
endmodule

module delay_tb4#(
	parameter int NUM_TESTS = 1000,
	parameter int CYCLES = 4,
	parameter int WIDTH = 8
);
	
	logic clk = 1'b0, rst, en;
	logic [WIDTH-1:0] data_in, data_out;
	
	delay #(
		.CYCLES(CYCLES),
		.WIDTH(WIDTH)
	) DUT (
		.*
	);
	
	initial begin : clk_gen
		forever #5 clk <= ~clk;
	end
	
		initial begin : stim_proc
		$timeformat(=9, 0, " ns");
		
		rst <= 1'b1;
		en <= 1'b0;
		data_in <= '0;
		repeat (5) @(posedge clk);
		@(negedge clk);
		rst <= 1'b0;
		
		for (int i = 0; i < NUM_TESTS; i++) begin
			data_in <= $urandom;
			en <= $urandom;
			@(posedge clk);
		end
		
		disable clk_gen;
		disable check_output;
		$display("Tests completed.");
	end
		// new weird construct that's awesome. what we've been waiting for is that the enable has been asserted for at least CYCLES number of cycles, so we can check for that with the [-> CYCLES] construct, which will be the antecedent of the implication before it checks for the consequence. 
	// one thing to note, the amount of times a signal is passed in and required to be asserted X number of times does NOT need to be consecutive. so in this instance it can be 0 1 0 1 0 1 0 0 0 0 1 and on that last 1, it will start the assertion.
	// one thing to note about assertions is that all the variables inside it are sampled at the clock edge defined, so clk, en, data_out, data_in, CYCLES, en. BUT the thing with disable is that it isn't sampled the same time as the clock edge, so if it changes in between the clock edge weird things can happen.
	// this is why complex disable conditions may be bad and shouldn't be used, as it can cause behavior that will waste time. sometimes don't even use a disable statement so that sampling is consistent. 
		assert property (@(posedge clk) disable iff (rst) en [-> CYCLES] |-> data_out == $past(data_in, CYCLES, en)); 
		// now, we want to ensure that when the reset clears, data_out is 0 for CYCLES cycles
		assert property (@(posedge clk) $fell(rst) |-> data_out == '0 throughout en [-> CYCLES]); 
		// $fell() checks for if something asserted the sample before is no longer asserted
		// throughout will make sure data_out is '0 for a window of time defined by the [-> #] construct. a little complicated. "throughout CYCLES cycles when en is asserted, we need to assert that data_out is 0"
		
		assert property (@(posedge clk) rst |=> data_out == '0);
		assert property (@(posedge clk) disable iff (rst) !en |=> $stable(data_out));
// all these assertions are basically what we want to do. the throughout and [->] construct may be a bit confusing, so however powerful, it requires an informed reader to understand what's going on. 	
endmodule


moduke simple_pipeline_with_en_tb#(
	parameter int NUM_TESTS = 1000,
	parameter int WIDTH = 8
);

	logic clk, rst, en, valid_in, valid_out;
	logic [WIDTH-1:0] data_in[8], data_out;
	
	simple_pipeline_with_en #(.WIDTH(WIDTH)) DUT (.*);
	
	initial begin : clk_gen
		forever #5 clk <= ~clk;
	end
	
	initial begin : stim_proc
		$timeformat(-9, 0, " ns");
		rst <= 1'b1;
		en <= 1'b0;
		valid_in <= 1'b0;
		data_in <= {default: '0};
		repeat (5) @(posedge clk);
		@(negedge clk);
		rst <= 1'b0;
		@(posedge clk);
		
		// run tests
		for (int i = 0; i < NUM_TESTS; i++) begin
			en <= $urandom;
			for (int j = 0; j < 8; j++) data_in[j] <= $urandom;
			valid_in <= $urandom;
			@(posedge clk);
		end
		
		disable clk_gen;
		@display("Tests completed.");
	end
	
	function automatic logic is_out_correct(logic [WIDTH-1:0] data_in[8]);
		logic [WIDTH-1:0] sum = 0;
		for (int i = 0; i < 4; i++) sum += data_in[i*2] * data_in[i*2+1];
		returm sum == data_out; // data out being stimmed by DUT from stim_proc
	endfunction

	function automatic logic is_out_correct(logic [WIDTH-1:0] data_in[8], logic [WIDTH-1:0] data_out);
		logic [WIDTH-1:0] sum = 0;
		for (int i = 0; i < 4; i++) sum += data_in[i*2] * data_in[i*2+1];
		returm sum == data_out; // data out being stimmed by DUT from stim_proc
	endfunction
	
	// technically whitebox testing, which isn't recommended in verification world. we're using a defined latency in order to make our assertion easier
	// assert property (@(posedge clk) disable iff (rst) en [-> DUT.LATENCY] |=> data_out == is_out_correct($past(data_in, DUT.LATENCY, en)));
	// this will give a bunch of errors! why? sampling issues persist. if we looked at the waveform, we can see that the data_out *should* be matching, but  it's actually changing instantly when the assertion is made. in order to change this, we edit the function to take in BOTH data_in and data_out so it's at the same sample time and won't change before the assertion is made. 
	assert property (@(posedge clk) disable iff (rst) en [-> DUT.LATENCY] |=> data_out == is_out_correct($past(data_in, DUT.LATENCY, en), data_out));
	// now this works! went from bad to less bad
	assert property (@(posedge clk) disable iff (rst) en [-> DUT.LATENCY] |-> valid_out == $past(valid_in, DUT.LATENCY, en)); 
	// we're checking to make sure our valid_out is asserted LATENCY cycles after valid_in is asserted :thumbsup:
	assert property (@(posedge clk) $fell(rst) |-> !valid_out throughout en [-> DUT.LATENCY]);
	// now we want to make sure that when reset falls, we have DUT.LATENCY amount of 0's for valid out to ensure the pipeline is working properly and not pushing a valid signal too early. 
	assert property (@(posedge clk) $fell(rst) |-> data_out == '0 throughout en [-> DUT.LATENCY]);
	// lets do the same thing but for the output since the reset should clear all values in the pipeline
	assert property (@(posedge clk) !en |=> $stable(data_out) && @stable(valid_out)); 
	// now we make sure that when the pipeline isn't enabled, everything is stable and stays the same
	assert property (@(posedge clk) rst |=> data_out == '0);
	assert property (@(posedge clk) rst |=> !valid_out);
	// now we verify the functionality of our reset and we're done with this testbench!
	
	// one of the crazy thigns here is that as long as the pipelined circuit has a valid_in/out, data that flows in and out, we can modify the model and reuse A LOT of this code in order to test any pipelined circuit that we want. 
endmodule


moduke simple_pipeline_with_en_tb#(
	parameter int NUM_TESTS = 1000,
	parameter int WIDTH = 8
);
	import dut_pkg::latency;
	localparam LATENCY = dut_pkg::latency(); // make a package so we're blackbox testing and we just know the latency of the pipeline
	logic clk, rst, en, valid_in, valid_out;
	logic [WIDTH-1:0] data_in[8], data_out;
	
	simple_pipeline_with_en #(.WIDTH(WIDTH)) DUT (.*);
	
	initial begin : clk_gen
		forever #5 clk <= ~clk;
	end
	
	initial begin : stim_proc
		$timeformat(-9, 0, " ns");
		rst <= 1'b1;
		en <= 1'b0;
		valid_in <= 1'b0;
		data_in <= {default: '0};
		repeat (5) @(posedge clk);
		@(negedge clk);
		rst <= 1'b0;
		@(posedge clk);
		
		// run tests
		for (int i = 0; i < NUM_TESTS; i++) begin
			en <= $urandom;
			for (int j = 0; j < 8; j++) data_in[j] <= $urandom;
			valid_in <= $urandom;
			@(posedge clk);
		end
		
		disable clk_gen;
		@display("Tests completed.");
	end
	
	function automatic logic is_out_correct(logic [WIDTH-1:0] data_in[8]);
		logic [WIDTH-1:0] sum = 0;
		for (int i = 0; i < 4; i++) sum += data_in[i*2] * data_in[i*2+1];
		returm sum == data_out; // data out being stimmed by DUT from stim_proc
	endfunction

	function automatic logic model(logic [WIDTH-1:0] data_in[8]);
		logic [WIDTH-1:0] sum = 0;
		for (int i = 0; i < LATENCY; i++) sum += data_in[i*2] * data_in[i*2+1];
		returm sum;
	endfunction
	
	// this is good, but we have to consider the case where our input data is NOT valid. this has asserted that we're gonna get a clean flow of data, no problemos
	assert property (@(posedge clk) disable iff (rst) en [-> LATENCY] |=> data_out == model($past(data_in, LATENCY, en)));
	assert property (@(posedge clk) disable iff (rst) valid_in |-> en [-> LATENCY] ##1 data_out == model($past(data_in, LATENCY, en)));
	// the first one will trigger every latency for data_out, the second one will check for only when valid_in is asserted.
	assert property (@(posedge clk) disable iff (rst) en [-> LATENCY] |=> valid_out == $past(valid_in, LATENCY, en));
	
	// this is a good way to test in general, but we don't always have to look back in time, instead, we can carry values with us
	
	property check_outout_p;
		logic [WIDTH-1:0] correct_data_out;
		@(posedge clk) disable iff (rst) (valid_in && en, correct_data_out = model(data_in)) |-> en [-> LATENCY] ##1 data_out == correct_data_out;
		// if valid in, calculate correct data of current data_in, then wait LATENCY clock cycles, and on the next one assert data_out is correct. this way we don't have to look back in time and we can carry a value with us. we can extend sequences like crazy, but at that point you might wanna split it up. 
	endproperty
	assert property (check_output_p);	
endmodule
```
# Coverage
```systemverilog
module fifo_tb#(
	parameter int NUM_TEST = 1000,
	parameter int WIDTH = 8,
	parameter int DEPTH = 16
);

	logic clk = 1'b0, rst, full, wr_en, empty, rd_en;
	logic [WIDTH-1:0] wr_data, rd_data; 
	
	fifo #(.WIDTH(WIDTH), .DEPTH(DEPTH)) DUT (.*);
	
	initial begin : clk_gen
		forever #5 clk <= ~clk;
	end
	
	inital begin : stim_proc
		rst <= 1'b1;
		rd_en <= 1'b0;
		wr_en <= 1'b0;
		wr_data <= '0;
		repeat (5) @(posedge clk);
		@(negedge clk);
		rst <= 1'b0;
		
		for (int i = 0; i < NUM_TESTS; i++) begin
			wr_data <= $urandom;
			wr_en <= $urandom;
			rd_en <= $urandom;
		end
		
		disable clk_gen;
		$display("Tests completed.");
	end
	
	assert property (@(posedge clk) DUT.valid_wr |-> !full);
	assert property (@(posedge clk) DUT.valid_rd |-> !empty);
	
	logic [WIDTH-1:0] correct_rd_data;
	logic [WIDTH-1:0] model_queue[$];
	
	// model the functionality of the FIFO with a queue
	always_ff @(posedge clk or posedge rst) begin
		if (rst) model_queue = {};
		else begin
			automatic int size model_queue.size();
			if (rd_en && size > 0) correct_rd_data <= model_queue.pop_front();
			if (wr_en && size < DEPTH) model_queue.push_back(wr_data);
		end
	end
endmodule




```











