# elevated_shell
JavaScript snippet to create an elevated shell

1. Copy code from shell.js into your code.
2. Create new shell object in your code: `const shell = new Shell("root", "sh", "password")`. This can also be done in a function. The `shell` field does not effect the shell at the time.
3. Call `shell.write("echo 'test'\n")` to see if it is working or run a command. Always make sure to have a terminating `\n`.
4. Modify the class if you want to get the `data` of stderr and stdout somewhere else
5. Star (optional)

## Example
```lang=js
const chpr = require("child_process")

class Shell {
	constructor(username, shell, password) {
		this.username = username
		this.shell = shell
		this.password = password
		this.s_process = chpr.exec(`su ${username}\n`)

		this.s_process.stdin.write(this.password+"\n")
		this.s_process.stderr.on("data", (data) => {console.log("Shell Err: " + data)})
		this.s_process.stdout.on("data", (data) => {console.log("Shell Out: " + data)})
	}
	write(command) {
		this.s_process.stdin.write(command)
		console.log("Shell In: " + command)
	}
}

const shell = new Shell("root", "sh", "root_password")
shell.write("echo 'You see this in your terminal, you did it'\n")

```
