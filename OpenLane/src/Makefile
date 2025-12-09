# Makefile para compilar y simular Verilog en src/

# Archivos fuente
SRC = femto.v
TOP = femto   # módulo top-level

# Archivos de salida
OUT = simv
WAVES = waves.vcd

# Compilación con Icarus Verilog
all: $(OUT)

$(OUT): $(SRC)
	iverilog -o $(OUT) -s $(TOP) $(SRC)

# Ejecutar simulación
run: $(OUT)
	./$(OUT)

# Ejecutar simulación y generar ondas
waves: $(OUT)
	./$(OUT)
	gtkwave $(WAVES) &

# Limpiar archivos generados
clean:
	rm -f $(OUT) $(WAVES)

