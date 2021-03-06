#
# This code should work on *nix systems with Java SDK 6+.
# Run "make download" to resolve dependencies (download jars).
# Run "make" to build YAPG and the 101 example.
# Run "make bootstrap" to rebuild YAPG with itself.
#

cp = ".:../jars/antlr-3.2.jar:../jars/antlr-runtime-3.2.jar:../jars/junit-4.11.jar:../jars/hamcrest-core-1.3.jar"
java = java -cp ${cp}
javac = javac -cp ${cp}
antlr = java -cp ${cp} org.antlr.Tool
yapg = java -cp ${cp} org.softlang.yapg.Tool

# Build and test everything but bootstrapping
all:
	make build
	make test-company

# Download all jars from Maven repo
download:
	cd ..; \
	mkdir -p jars; \
	cd jars; \
	curl http://www.antlr3.org/download/antlr-3.2.jar -o antlr-3.2.jar; \
	curl http://repo1.maven.org/maven2/junit/junit/4.11/junit-4.11.jar -o junit-4.11.jar; \
	curl http://repo1.maven.org/maven2/org/hamcrest/hamcrest-core/1.3/hamcrest-core-1.3.jar -o hamcrest-core-1.3.jar

# Build the YAPG tool, test it for "101"
build:
	make build-yapg
	make build-company

# Build the YAPG tool
build-yapg:
	make antlr-yapg
	make compile-yapg

# Build the 101companies System including an application of YAPG
build-company:
	make yapg-company
	make antlr-company
	make compile-company

# Run ANTLR generator for parser of YAPG
antlr-yapg:
	${antlr} org/softlang/grammar/Grammar.g
	rm -rf *.tokens

# Compile YAPG
compile-yapg:
	${javac} org/softlang/grammar/*.java
	${javac} org/softlang/yapg/*.java

# Run YAPG generator for parser of 101companies System
yapg-company:
	${yapg} org/softlang/company org.softlang.company Company

# Run ANTLR generator for parser of 101companies System
antlr-company:
	${antlr} org/softlang/company/Company.g
	rm -rf *.tokens

# Compile classes of 101companies System
compile-company:
	${javac} org/softlang/company/*.java
	${javac} org/softlang/tests/*.java

# Test parser of 101companies System
test-company:
	${java} org.junit.runner.JUnitCore org.softlang.tests.Parsing

# Remove all files that can be derived
clean:
	make clean-company
	make clean-yapg

# Remove all files of 101companies implementation that can be derived
clean-company:
	rm -f org/softlang/company/*.class
	rm -f org/softlang/tests/*.class
	rm -f org/softlang/company/*.g
	rm -f org/softlang/company/*.java
	rm -rf org/softlang/company/*.tokens

# Remove all files of YAPG implementation that can be derived
clean-yapg:
	rm -rf ../jars
	rm -f org/softlang/grammar/*.class
	rm -f org/softlang/yapg/*.class
	rm -f org/softlang/grammar/GrammarLexer.java
	rm -f org/softlang/grammar/CompanyParser.java
	rm -rf org/softlang/grammar/*.tokens

#
# Apply YAPG to itself.
# That is, generate .g and .java in org/softlang/grammar/*
#
bootstrap:
	${yapg} org/softlang/grammar org.softlang.grammar Grammar
