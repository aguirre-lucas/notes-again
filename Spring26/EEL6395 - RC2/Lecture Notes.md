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
	assign out = sel == 1'b1 /* could also just be `sel` */* ?  in1: in0;
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
	parameter NUM_INPUTS
)(

);



endmodule
```