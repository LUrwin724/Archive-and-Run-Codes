# Archive-and-Run-Codes

Overview of Functions
	These codes are for the purpose of streamlining the use of the BOUT++ simulation code and the filing of run data. 
	Currently these codes are optimised for use with the SOL1D example case and many of the settings are set by default 
	to match this example simulation of the code. Console is a program for use within ipython to manipulate an archive 
	folder of input files and data files for use with the BOUT++ code, whilst runbout is designed to be run from the Unix 
	command line to run simulations of specified folders within the archive. The scanbout is an extension of the runbout 
	code to repeatedly run the code whilst changing 1 or 2 specified parameters.

Installation
	There are 5 files in total for using the outlined functions: console.py, runbout.py, scanbout.py, inputs.txt, BOUT.inp 
	and config.ini. An archive folder should be created in a location where the user would like to store their files. 
	Inside this a config folder should be created storing the config.ini file, a default BOUT.inp file and if desired an 
	inputs.txt file which is used for a function within console.py. The three python codes can again be stored anywhere 
	but it is useful to be stored within a folder in the python libraries of the BOUT++ code. The runbout and scanbout 
	codes may require a conversion to Unix format due to being written in windows. This can be quickly resolved by running 
	dos2unix “code.py” within the command line. They then are designed to be made into executable files via writing 
	chmod u+x “code.py” to the command line. Within the console.py, runbout.py and scanbout.py codes is a line directly 
	below the importation of python libraries that requires editing with the required path to the config.ini file. The 
	sections within config.ini file should then be edited with the desired paths or functions. Further details are below 
	in the specific documentation for the config.ini file.



Config.ini
	 Within the config.ini files are the following subheadings used for definitions of the programs to be read by ConfigParser: 

[exe] - is used to define the path to the executable of the BOUT++ simulation case that is required to be run the 
corresponding input files.

[archive] - should be set to the path location of the archive folder.

[text editor] - gives the user the option of the automatic opening of archive text files within the desired editor.

[BOUT.inp keys] and [BOUT.inp subkeys] are used to define error messages for the manipulation of the BOUT.inp file and 
should be edited to correlate to the input files being used for different examples within the BOUT++ code.



Console.py

	The console.py program is designed to be run in ipython such that it can allow the user to be used alongside the 
	collection and plotting of data from the boutdata and boututils python libraries of BOUT++. Within ipython 
	from ‘python library’.console import * will load all the functions defined within the program for use of manipulating 
	the python library.

	 Additional .ini files (usernotes and record) are created by the console.py program to document the archive and will 
	 accompany BOUT.inp files as they are editied etc. Usernotes are edited manually in the format of the first line being 
	 a summary of the folder and the rest of the file, more extensive notes may be written if necessary. The record.ini 
	 files are an automated history of a BOUT.inp file; how and when it has been edited and its parent files.

list_archive(‘filetype’) - prints all files of a defined extension to the screen with times of creation and the first 
line of the corresponding usernotes.ini file. Default if no file type given is ‘inp’.

load( ) - lists all BOUT.inp input files within the archive and allows user to print the contents of the chosen file 
to screen.

create( ) - creates a new folder of a user defined name within the archive and uploads the default BOUT.inp files from 
the config folder to the new folder.

compare( ) - lists all BOUT.inp input files and prints out the differences of two chosen files to screen. A +/– with 
nothing followed indicates an additional linespace in one of the files. 

edit( ) - lists all BOUT.inp input files and allows the user to change the value of a setting (subkey) within the file 
corresponding to a chosen key heading. If the subkey has no heading the key may be left blank (such as NOUT or TIMESTEP).

edit_create( ) - creates a new file into which the change is saved, leaving chosen file to edit unchanged and gives 
the option of saving the runfiles to the new folder to allow restarts.

inputs( ) -  prints the inputs.txt file to screen to find keys and subkeys for edit functions. The input.txt file would 
require changing to the format of the input code being used. Default is the SOL1D case.

edit_notes( ) - lists all the .ini files  in the archive and opens chosen file in a text editor.

copy_all( ) - copies all files from one archive folder into another.

copy_runfiles( ) - copies all restart and log files from one archive folder into another.

change_inputs( ) -  changes the inputs.txt and config.ini files to have the correct keys and subkeys for the chosen input 
file from the archive. Use each time different BOUT++ examples are used.

Miscellaneous error functions are defined at the bottom of the code to prevent typos messing with the formatting of the 
BOUT.inp files. These link to the [BOUT.inp keys] and [BOUT.inp subkeys] of the config file which require editing if 
different formats of input files are used for different simulation examples.



Runbout.py

 	After installation the runbout.py code can be used from the command line to run any BOUT.inp files within the archive 
 	with the option of using any restart files in the folder. The format of the command is: ./runbout.py sys.argv[1], where 
 	sys.argv[1] is the name of the folder of the archive you wish to run. The default progression of the code is to then 
 	create a temporary directory ‘tmp’ in the folder of runbout.py and copy all files from the chosen archive folder into 
 	tmp to use as a run directory. The code then uses the launch.py program (which may be edited to use a nice -10 command) 
 	from boututils to run BOUT++. If the run is successfully completed the files from the tmp files are saved and overwrites 
 	the files in the archive folder and the tmp directory is deleted and the option to edit usernotes.ini is given.

	Current NB: whilst the tmp folder is wiped upon a user exit of the run, it is not for a system exit. The tmp folder is 
	then required to manual deletion to avoid any interference with later runs due to e.g a different n.o of restart files. 
	Also currently the default number of processers to run on is set to 5, but the user input of nproc can be uncommented 
	and the run cmd can be quickly edited to change this.



Scanbout.py

	The Scanbout code works exactly the same as the runbout.py code with the addition of doing multiple runs, changing 
	the input file as each run is completed. The syntax to run the code is the same as the runbout.py command. The user 
	is given the choice of scanning 1 or 2 variables with the option of using the restart files of the last iteration as 
	the scan is performed. The user is required to give the key and subkey of the variable(s) they wish to scan, same as 
	the edit( ) function. An initial value is taken from the BOUT.inp file loaded for each variable whilst the corresponding 
	increment and the limit of the scan are user defined. The option of how to define the increment is given before the 
	defining the increment on whether the increment adds a set number to the previous iteration or increases it by a set 
	percentage given as a decimal. For each iteration of the scan the output data are saved to folders within the folder 
	loaded from the archive and given names corresponding to the subkeys and iteration value of the subkey. Error codes 
	are defined at the bottom of the code to ensure correct keys and subkeys are entered and approximately sensible values 
	for the increments and limits are entered.

	There are two different options for scanning two variables. One is to scan each permutation of the different values 
	achievable from the limits and increments given. The other (twin) is to increase them in tandem with the limits being 
	defined by the first parameter. 

NB: In addition to the current notes regarding the runbout program, the restart function cannot be used as a continuation 
of each run unless there are already restart files within the folder loaded from the archive. Also currently for scanning 
two variables (permutations), the value of the 1st variable is reset for each change of the 2nd variable. Hence large 
changes may cause restart files of the last iteration to cause instability. To reduce this, the restart files of the archive 
folder are reloaded each time the 2nd variable is changed and therefore the 1st variable is reset. Hence it is recommended 
that the 2nd variable is scanned over a few small increments.
