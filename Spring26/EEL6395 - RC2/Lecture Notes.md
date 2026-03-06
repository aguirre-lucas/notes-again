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
```














