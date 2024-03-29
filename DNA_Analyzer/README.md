# DNA Analyzer

## Contents

* [Overview](#Overview)
	* [Text Files](#Text-Files)
	* [Menu](#Menu)
* [Program Actions](#Program-Actions)
	* [Copy DNA](#Copy-DNA)
	* [DNA Scores](#DNA-Scores)
		* [Shift Scores](#Shift-Scores)
		* [Codon Scores](#Codon-Scores)
* [Unit Tests](#Unit-Tests)
	* [VS-2019](#Visual-Studio-2019)
* [Demonstration](#Demonstration)

## Overview

This repository contains a **C** program which opens a <em>.txt</em> file to analyze a list of DNA sequences, by scoring each of them against a DNA sample sequence. The DNA sample must be at most the same length as the DNA sequence.

### Text Files

The formatted DNA sequence file can be chosen from the following.

* [(`short_sample.txt`)](CPSC259_Lab2_Takehome/short_sample.txt)
* [(`long_sample.txt`)](CPSC259_Lab2_Takehome/long_sample.txt)
* [(`perfect_match.txt`)](CPSC259_Lab2_Takehome/perfect_match.txt)

There are also corresponding results contained in a <em>.txt</em> file of name format <em>x_result.txt</em> where <em>x</em> is the name of the formatted DNA sequence file.

### Menu

The program menu is implemented in the [(`main.c`)](CPSC259_Lab2_Takehome/main.c) source file. An infinite loop in the `int main();` function is used to prompt the user for the next program action.

## Program Actions

The user may choose to :</br>

<ol>
	<li>Load a <em>.txt</em> file.</li>
	<li>Score DNA sequences in <em>.txt</em> file.</li>
	<li>Exit the <b>C</b> program.</li>
</ol>

The selection is acquired from the standard input terminal.

### Copy DNA

We dynamically allocate memory for the DNA sequences :</br>

<ul>
	<li><em>char*</em> pointer to reference the sample segment</li>
	<li><em>char**</em> pointer to reference multiple candidate segments</li>
</ul>

The file is iterated to copy the contents to the respective memory location. This is done through extensive memory reallocation in the </br>`int extract_dna(...);` function, which is implemented in the [(`dna.c`)](CPSC259_Lab2_Takehome/dna.c) source file.

We must deallocate this memory and dereference the pointers when the user wishes to open a new <em>.txt</em> file.

### DNA Scores

We iterate through the candidates and score each of them against the sample sequence, unless a perfect match is found.

The best DNA scores for the candidate sequences are printed to the standard output terminal.

#### Shift Scores

We only consider the best DNA score for each candidate sequence. This is done through shifting the candidate sequence `char*` pointer leftwards by a codon length. We calculate the shift score by comparing the effective candidate sequence against the sample.

Given :</br>

<ul>
	<li>Sample Sequence : "AAAGGG"</br></li>
	<li>Candidate Sequence : "CTCAAAGGGTAT"</br></li>
</ul>

We Calculate Shift Scores For:</br>

<ol>
	<li>Effective Candidate Sequence: <em>"CTCAAAGGGTAT"</em></li>
	<li>Effective Candidate Sequence : <em>"AAAGGGTAT"</em></li>
	<li>Effective Candidate Sequence : <em>"GGGTAT"</em></li>
</ol>

Shift scores are calculated until the effective candidate sequence contains the same number of codons as the sample sequence.

#### Codon Scores

Codons are sequences composed of 3 nucleotides, such as <em>"CGG"</em>, <em>"ATT"</em>, <em>"GGC"</em>.

The basis of our scoring algorithm is the analysis of codons. Since codons are used to represent amino acids, score points are awarded for certain characteristics.

These include:</br>
<ul>
	<li>Base Pair Nucleotides : 1 Point</li>
	<li>Identical Nucleotides : 2 Points</li>
	<li>Same Amino Acids : 5 Points</li>
	<li>Identical Codons : 10 Points</li>
</ul>

The sum of the codon scores is equal to the DNA score for the effective candidate sequence. This is what we have defined as a shift score.

## Unit Tests

We also test the scoring functionalities through various test cases implemented in the [(`unittest.cpp`)](CPSC259_Lab2_UnitTests/unittest.cpp) file.

We run all tests on the different functions implemented in the [(`dna.c`)](CPSC259_Lab2_Framework/dna.c) source file. The development of these test cases was a time extensive process, in which edge case coverage was prioritized.

In our **C++** file test methods, we compare the calculated DNA scores with the actual scores from the <em>.txt</em> files with the following statement: </br>
`Assert::AreEqual(actual_score, calculated_score);`

### Visual Studio 2019

We must run our tests in the <b>Test Explorer</b> window of <b>Visual Studio 2019</b>. If all tests run as intended, the <b>Test Explorer</b> generates an output as shown below :

<p align="center">
    <img src="Figures/Unit_Tests.JPG" width="75%" height="75%" title="C++ Unit Tests for DNA Analyzer." >
</p>

This requires us to include the **C++** unit tests in a <b>Native Unit Test Project</b>.

We added a <b>Reference</b> from the [(`CPSC259_Lab2_UnitTests`)](CPSC259_Lab2_UnitTests/CPSC259_Lab2_UnitTests.vcxproj) project to the
[(`CPSC259_Lab2_TakeHome`)](CPSC259_Lab2_TakeHome/CPSC259_Lab2_TakeHome.vcxproj) project.

<b>Configuration Properties</b> in the <b>VS Solution Explorer</b> :

<ul>
    <li><b>Configuration Properties->General->Configuration Type</b> :</br> <em>Dynamic Library(.dll)</em></li>
    <li><b>VC++ Directories->General->Include Directories</b> :</br> <em>$(SolutionDir)\CPSC259_Lab2_Takehome;$(IncludePath)</em></li>
    <li><b>C/C++->General->Additional Include Directories</b> :</br> <em>$(SolutionDir)\CPSC259_Lab2_Takehome;$(VCInstallDir)UnitTest\include;%(AdditionalIncludeDirectories)</em></li>
    <li><b>C/C++->Preprocessor->Preprocessor Definitions</b> :</br> <em>WIN32;_DEBUG;%(PreprocessorDefinitions)</em></li>
    <li><b>C/C++->Precompiled Headers->Precompiled Header</b> :</br> <em>Not Using Precompiled Headers</em></li>
    <li><b>Linker->General->Additional Library Directories</b> :</br> <em>$(SolutionDir)\CPSC259_Lab3_Takehome\Debug;$(VCInstallDir)UnitTest\lib;%(AdditionalLibraryDirectories)</em></li>
    <li><b>Linker->Input->Additional Dependencies</b> :</br> <em>dna.obj;%(AdditionalDependencies)</em></li>
</ul>

### Demonstration

A video in the [`Demonstrations`](Demonstrations) directory shows the DNA analyzer when running the program on <b>Visual Studio</b>. I have embedded a low resolution compressed version below.


https://user-images.githubusercontent.com/52113009/135697887-fd3978af-ad3b-4cd2-8e92-4ff7584d32d3.mp4
