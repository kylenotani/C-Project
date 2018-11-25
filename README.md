# TurtleGraphics
C Project for University.

Reads a series of commands and draws to the terminal.

An input file containing a series of commands is validated strictly before insertion into a linked list data structure and drawn to the terminal. The validation focuses on a single line within the file expected to match a command’s name, it’s correct datatype, parameters as well as the value’s range. If validation fails, the file is not valid. The user is given an indication of what validation criteria failed, specifically the line number and type of validation failure that occurred. If all validation failures are fixed, each command will be stored as a single command struct with its name and value to be inserted at the end of the linked list. Validation involves a precedence of checks, in which the command name must be valid first to validate the number of parameters, the datatype and value range. Once a command name is found after calling validateCommandName(), pointers to each of the three validation functions depending on the command are set as exports to be used within main.

Once all validated commands from the file is read into the linked list, drawing will commence. During drawing, each list node pointing to a command data struct within the full list will be iterated through, starting its corresponding operation for each. The ordering of command operations to commence will be in the same order as in the input file based on the insertion of data. Achieving drawing to the output terminal requires a set of x and y coordinates to be calculated with trigonometry. The math library contains cos() and sin() functions each assist this to determine end coordinates based on the distance and angle from initial point (0, 0). A cosine/sine of a given angle in radians will be returned to provide the x and y coordinates from a given angle and distance. E.g.) From initial point (0,0) an angle of 270 degrees with a distance of 10 will calculate a new y coordinate on a 2D axis to the terminal.  (0, 10).

A given angle must be within 360 degrees to ensure valid coordinates are evaluated during drawing. After each command modifies the current angle, it is mod by 360 to attain this range. E.g.) Before mod 360 = -90, After mod 360 = 270. An issue with drawing results in double printing based on the nature of how drawing a line to a 2D coordinate space works. This is solved by ensuring that a draw to a given distance draws to a distance-1 and the start move adjusts the cursor to the intended start coordinates for the draw to continue from. The move and draw commands require the correct coordinates to successfully print commands to the terminal. 

During drawing, both command’s start and end coordinates are logged into a graphics.log file for debugging purposes. The rotate command adjusts the current angle within the space assuring as mentioned that the angle is within 360 degrees. The foreground command sets the foreground colour of the terminal from several colour values. The background command similarly sets the background colour of the terminal. Both are overridden once simple mode is enabled, disabling the feature to change colours while having the background colour force set to white (7) and foreground to black (0). The pattern command simply sets the drawing pattern to a single printable character. The state of each position, angle, pattern and foreground and background colours will be kept within a single ‘GraphicsState’ struct passed around to each command function.

Once all commands are read, the executed order of commands from the list results in a drawing on the terminal.
An alternative approach to converting the input file to a coordinate system is to delay validation until the drawing stage by immediately calling draw() and passing in read in line data. This approach dismisses storing all validated command data initially before drawing into a linked list data structure, by instead working on a single struct data and calling draw(). This will result in draw doing all the strict validation which involves command name, data type, parameter and value range checks as well as writing to the log file and choosing the command operation to commence upon valid commands. The current graphics state values can also be stored as local variables within the draw() function as opposed to being passing around within a struct. This approach lifts off the work that the main() function will do, however, draw() must deal with validation by checking each command operation, and writing and choosing the command operation to commence at a given line read. 

If a command is invalid operations are stopped and an error is printed to the screen only detailing the validation errors that occurred on a command at the given time. This means that all validated commands which have been successfully executed/printed on the terminal will be shown to the user until an error detailing an invalid command is output to the terminal. This approach allows the user to see how the drawing went before an invalid command was found. For each successful move/draw command that involves writing to the graphics.log file would also be appended upon single read stage.







