### **ROM Design (Basic)**

#### **Concept Overview**  
ROM (Read-Only Memory) is a non-volatile memory that stores data permanently and allows **read-only access**. It is widely used for storing fixed data like firmware, lookup tables, and initialization routines in digital systems.

#### **Detailed Explanation**  
- **Inputs:**  
  - `addr`: Address input to select memory location  
  - `clk`: Clock input (optional, if synchronous ROM)  

- **Output:**  
  - `dout`: Data output corresponding to the selected address  

- **Key Characteristics:**  
  - Data is **predefined** and cannot be altered during runtime  
  - Can be implemented using case statements or memory arrays initialized with `$readmemh` or `$readmemb`  

#### **Applications**  
- Microcontroller firmware  
- Character generators in display systems  
- Constant coefficient storage in DSP  

#### **Execution Steps**  
1. Define ROM size and data width.  
2. Preload values into ROM using `initial` block or file loading.  
3. Use `always @(*)` for combinational read, or `always @(posedge clk)` for synchronous.  
4. Create a testbench to read from all address locations and verify output.

#### **Real-World Example for Practice**  
Design an 8x8 ROM storing predefined hexadecimal values and read them using address input.

#### **Example Verilog Code (8x8 ROM using case statement)**  
```verilog
module simple_rom (
    input [2:0] addr,
    output reg [7:0] dout
);
    always @(*) begin
        case (addr)
            3'b000: dout = 8'hA1;
            3'b001: dout = 8'hB2;
            3'b010: dout = 8'hC3;
            3'b011: dout = 8'hD4;
            3'b100: dout = 8'hE5;
            3'b101: dout = 8'hF6;
            3'b110: dout = 8'h07;
            3'b111: dout = 8'h18;
            default: dout = 8'h00;
        endcase
    end
endmodule
```

#### **Testbench Code**  
```verilog
module testbench;
    reg [2:0] addr;
    wire [7:0] dout;

    simple_rom uut (.addr(addr), .dout(dout));

    initial begin
        $display("Addr\tData");
        for (addr = 0; addr < 8; addr = addr + 1) begin
            #5 $display("%b\t%h", addr, dout);
        end
        $finish;
    end
endmodule
```

#### **Expected Output**
```
Addr    Data  
000     A1  
001     B2  
010     C3  
011     D4  
100     E5  
101     F6  
110     07  
111     18  
```

#### **Further Enhancements**  
- Load contents from external file using `$readmemh("rom_data.mem", mem);`  
- Implement parameterized ROM  
- Convert to synchronous ROM with `clk` input  
