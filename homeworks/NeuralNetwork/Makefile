DIR = ../../matlib
DLLS = $(addprefix -reference:,$(filter %.dll,$^))
Data = valPartA.data ANNresPartA.data
Data2 = ANNresInPartB.data ANNresDePartB.data ANNresDe2PartB.data ValInPartB.data ValDePartB.data ValDe2PartB.data valPartB.data
OUTPUTS = $(addprefix -output:,$(filter %.data,$(Data) $(Data2)))
MKLIBF = mcs $(filter %.cs,$^) -target:library -out:./$@
MKLIB = mcs $(filter %.cs,$^) -target:library -out:$@
MKEXE = mcs $< -target:exe -out:$@

Task: Out.txt partAplot.svg partBplot.svg partB1plot.svg partB2plot.svg

Out.txt: main.exe
	mono $< $(OUTPUTS) > $@
main.exe: main.cs ANN.dll matlib.dll IOput.dll
	$(MKEXE) $(DLLS)
$(Data) $(Data2):main.exe

matlib.dll: $(DIR)/matrix/*.cs $(DIR)/genlist.cs
	$(MKLIBF)
IOput.dll:$(DIR)/IOputs.cs matlib.dll
	$(MKLIBF) $(DLLS)
ANN.dll: ANN.cs matlib.dll IOput.dll
	$(MKLIB) $(DLLS)

partAplot.svg: partA.gpi $(Data)
	gnuplot $< > $@
partBplot.svg: partB.gpi ANNresInPartB.data ValInPartB.data
	gnuplot $< > $@
partB1plot.svg: partB1.gpi ANNresDePartB.data ValDePartB.data
	gnuplot $< > $@
partB2plot.svg: partB2.gpi ValDe2PartB.data ANNresDe2PartB.data
	gnuplot $< > $@
clean:
	rm -f *.svg *.dll *.exe *.data Out.txt
