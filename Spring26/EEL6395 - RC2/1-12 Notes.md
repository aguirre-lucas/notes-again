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
- 