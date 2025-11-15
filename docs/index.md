# NYNS (Not Yo' Normal Shell)

NYNS is a tiny, script‑driven shell written in Bash. Instead of typing
commands interactively, you write a **NYNS script file**, and NYNS executes
each line in order.

This site explains how to install NYNS, write NYNS scripts, and understand the
available commands.

## Getting NYNS

Clone the NYNS repository:

```bash
git clone https://github.com/eotter-beep/nyns
cd nyns
```

Make the main script executable:

```bash
chmod +x bin/nyns.sh
```

You can now run NYNS against a script file:

```bash
./bin/nyns.sh path/to/script.nyns
```

> NYNS reads the script line‑by‑line and ignores empty lines and comments
> (lines starting with `#`).

## Writing Your First NYNS Script

Create a file called `example.nyns` in the `nyns` folder:

```bash
echo hello world
+ 2 3
- 10 4
create myfile.txt
```

Run it with:

```bash
./bin/nyns.sh example.nyns
```

You should see:

- `hello world`
- `5`
- `6`
- and a new file named `myfile.txt` in the directory where you ran NYNS.

## Built‑in Commands

NYNS currently understands these commands:

- `echo <text1> <text2>` – print the two arguments as a line of text.
- `+ <a> <b>` – output the sum of `a` and `b`.
- `- <a> <b>` – output the result of `a - b`.
- `rem <path>` – delete a file or directory recursively (**dangerous**,
  uses `rm -rf`).
- `rem -f <path>` – force delete a file (`rm -f`).
- `moveto <path>` – change directory to `path` (like `cd`).
- `ip` – show IP address information (`ip addr`).
- `create <path>` – create an empty file (`touch`).
- `adm <cmd>` – run a command as root using `sudo` (be careful).
- `partition <device>` – run `fdisk` on the given device (**advanced and
  dangerous**).
- `help` – print a summary of available commands inside NYNS.

Any other first word on a line is treated as an unknown command and will
produce an error message.

## Example: Simple Setup Script

A slightly more realistic script might look like:

```bash
# Setup a project workspace
create notes.txt
echo Project created notes.txt
```

Run it with:

```bash
./bin/nyns.sh setup.nyns
```

## Safety Notes

Some NYNS commands wrap powerful system utilities:

- `rem`, `rem -f`, and `partition` can delete data or modify disks.
- `adm` runs commands with `sudo`, which can affect the whole system.

Only run scripts you trust and understand, and test on non‑critical systems
first.

## Contributing to NYNS and These Docs

- NYNS source code: <https://github.com/eotter-beep/nyns>
- Documentation source: <https://github.com/eotter-beep/nynsdocs>

You can improve NYNS itself, add new commands, or expand these docs with more
examples and guides. Pull requests are welcome.
