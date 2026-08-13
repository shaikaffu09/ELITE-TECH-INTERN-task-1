// basic_alu.v
module basic_alu (
    input  [3:0] A,
    input  [3:0] B,
    input  [2:0] ALU_Sel,
    output reg [3:0] Result,
    output reg       Carry
);

always @(*) begin
    // Default values
    Result = 4'b0000;
    Carry  = 1'b0;

    case (ALU_Sel)

        3'b000: begin
            // Addition
            {Carry, Result} = A + B;
        end

        3'b001: begin
            // Subtraction
            Result = A - B;
        end

        3'b010: begin
            // AND
            Result = A & B;
        end

        3'b011: begin
            // OR
            Result = A | B;
        end

        3'b100: begin
            // NOT A
            Result = ~A;
        end

        default: begin
            Result = 4'b0000;
            Carry  = 1'b0;
        end

    endcase
end

endmodule
// basic_alu_tb.v
module basic_alu_tb;

    reg  [3:0] A;
    reg  [3:0] B;
    reg  [2:0] ALU_Sel;

    wire [3:0] Result;
    wire       Carry;

    // Instantiate ALU
    basic_alu uut (
        .A(A),
        .B(B),
        .ALU_Sel(ALU_Sel),
        .Result(Result),
        .Carry(Carry)
    );

    initial begin

        // Create waveform dump
        $dumpfile("basic_alu.vcd");
        $dumpvars(0, basic_alu_tb);

        // Addition: 5 + 3 = 8
        A = 4'b0101;
        B = 4'b0011;
        ALU_Sel = 3'b000;
        #10;

        // Subtraction: 5 - 3 = 2
        A = 4'b0101;
        B = 4'b0011;
        ALU_Sel = 3'b001;
        #10;

        // AND: 5 & 3 = 1
        A = 4'b0101;
        B = 4'b0011;
        ALU_Sel = 3'b010;
        #10;

        // OR: 5 | 3 = 7
        A = 4'b0101;
        B = 4'b0011;
        ALU_Sel = 3'b011;
        #10;

        // NOT: ~5 = 10
        A = 4'b0101;
        B = 4'b0000;
        ALU_Sel = 3'b100;
        #10;

        $finish;
    end

    // Display results
    initial begin
        $monitor("Time=%0t | A=%b | B=%b | Sel=%b | Result=%b | Carry=%b",
                 $time, A, B, ALU_Sel, Result, Carry);
    end
