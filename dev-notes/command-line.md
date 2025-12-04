# Command Line / Terminal

## General Notes

Enable line wrap: `tput smam`
Disable line wrap: `tput rmam`

`npm run test 2>&1 | tee test-output.txt` - this will run the test script and save the output to a file

Finding a file with Terminal:
`find . -type f -iname "*string_in_file_name_to_search_for*"`

## Deleting unused `node_module` directories

`npx npkill` is a helpful tool specifically designed for this purpose.

This will launch an interactive interface that:

- Scans your system for node_modules folders
- Shows the size of each folder
- Lets you navigate with arrow keys and delete with spacebar
- Shows potential space savings
- Helps you avoid accidentally deleting active project dependencies

## Resources
