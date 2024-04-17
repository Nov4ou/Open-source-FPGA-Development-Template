NAME = blink

all: sim sint

sim: $(NAME)_tb.vcd

sint: $(NAME).bin

$(NAME)_tb.vcd: $(NAME).v $(NAME)_tb.v
	
	#-- Compilar
	iverilog -o $(NAME)_tb.out $(NAME).v $(NAME)_tb.v

	#-- Simular
	./$(NAME)_tb.out
	
	#-- Ver visualmente la simulacion con gtkwave
	gtkwave $(NAME)_tb.vcd $(NAME)_tb.gtkw &
	

$(NAME).bin: icesugar.pcf $(NAME).v 
	
	#-- Sintesis
	yosys -p "synth_ice40 -blif $(NAME).blif" $(NAME).v
	
	#-- Place & route
	arachne-pnr -d 5k -P sg48 -p icesugar.pcf $(NAME).blif -o $(NAME).txt
	
	#-- Generar binario final, listo para descargar en fgpa
	icepack $(NAME).txt $(NAME).bin

upload:
	icesprog $(NAME).bin

clean:
	rm -f *.bin *.txt *.blif *.out *.vcd *~

.PHONY: all clean