DIR = ../../matlib
DLLS = $(addprefix -reference:,$(filter %.dll,$^))
Data = measHiggs.data fitHiggs.data guessHiggs.data
OUTPUTS = $(addprefix -output:,$(filter %.data,$(Data)))
MKLIBF = mcs $(filter %.cs,$^) -target:library -out:./$@
MKLIB = mcs $(filter %.cs,$^) -target:library -out:$@
MKEXE = mcs $< -target:exe -out:$@
USEDLLS = matrix.cs vector.cs

Task: Out.txt partBplot.svg

Out.txt: main.exe
	mono $<  < higgs.data.txt $(OUTPUTS) > $@
main.exe: main.cs minimi.dll matlib.dll IOput.dll
	$(MKEXE) $(DLLS)
$(Data):main.exe

matlib.dll: $(DIR)/matrix/matrix.cs $(DIR)/matrix/vector.cs  $(DIR)/genlist.cs
	$(MKLIBF)
IOput.dll:$(DIR)/IOputs.cs matlib.dll
	$(MKLIBF) $(DLLS)
minimi.dll: minimi.cs matlib.dll
	$(MKLIB) $(DLLS)

partBplot.svg: partB.gpi $(Data)
	gnuplot $< > $@

clean:
	rm -f *.svg *.dll *.exe *.data Out.txt
