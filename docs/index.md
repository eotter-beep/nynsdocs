# NYNS (Not Yo' Normal Shell)

NYNS is a tiny, script‑driven shell. Instead of typing commands interactively,
you write a **NYNS script file**, and NYNS executes each line in order. NYNS
started as a Bash script and now also has a C++ implementation (`nyns.cpp`)
with minimal external dependencies.

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

You can also build and use the C++ interpreter (`bin/nyns_cpp`) from
`bin/nyns.cpp`; see **C++ NYNS Interpreter (`nyns.cpp`)** below for details.

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

NYNS currently understands these commands (implemented in both the Bash
script and the C++ interpreter):

- `echo <text1> <text2>` – print the two arguments as a line of text.
- `+ <a> <b>` – output the sum of `a` and `b`.
- `- <a> <b>` – output the result of `a - b`.
- `rem <path>` – recursively delete a file or directory (**dangerous**).
- `rem -f <path>` – recursively delete a path and ignore “not found” errors.
- `moveto <path>` – change directory to `path` (like `cd`).
- `ip` – show IP address information (IPv4/IPv6 per interface).
- `create <path>` – create an empty file.
- `adm <cmd>` – run a command as root; only works if NYNS itself is running
  with root privileges (no `sudo` wrapper).
- `partition <image> [clean|add]` – on the Bash side wraps `fdisk` on a
  device; in the C++ build operates on a disk image file and can show the MBR
  partition table, wipe it (`clean`/`wipe`), or add a single primary
  partition (`add`).
- `help` – print a summary of available commands inside NYNS.
- `import <script.nyns>` – run another NYNS script file from within the
  current script.

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

- `rem` and `rem -f` can delete data recursively.
- In the Bash script, `adm` runs commands with `sudo` and `partition` wraps
  `fdisk`, both of which can affect disks and the whole system.
- In the C++ build, `adm` only works when NYNS is run as root, and
  `partition` can both inspect and modify the MBR of a disk image: it can
  wipe the partition table or create a single primary partition by writing
  directly to the image.

Only run scripts you trust and understand, and test on non‑critical systems
first.

## C++ NYNS Interpreter (`nyns.cpp`)

Alongside the Bash implementation, NYNS also has a C++ port intended for
environments where a full shell is not available (for example, low‑level
OS/boot tooling or BIOS‑oriented experiments).

- Source location: `bin/nyns.cpp` in the main NYNS repo.
- Implements the same core commands described above (`echo`, `+`, `-`, `rem`,
  `moveto`, `help`, `ip`, `create`, `adm`, `partition`, `import`).
- Runs NYNS scripts line‑by‑line just like `nyns.sh`, including nested scripts
  via `import`.
- Implements `ip` directly via networking APIs (no `ip` binary), requires
  NYNS to be run as root for `adm`, and implements `partition` as a small
  MBR tool for disk images: it can print the current partition entries, wipe
  the table, or add a single primary partition without calling `fdisk`.

To build the C++ interpreter on a typical Linux system:

```bash
cd nyns
g++ -std=c++17 -O2 -o bin/nyns_cpp bin/nyns.cpp
```

You can then execute a script with:

```bash
./bin/nyns_cpp path/to/script.nyns
```

When experimenting with OS development or BIOS‑level workflows, you can
cross‑compile `nyns.cpp` for your target environment and, if needed, replace
its remaining OS‑specific calls with equivalents for your platform.

## Contributing to NYNS and These Docs

- NYNS source code: <https://github.com/eotter-beep/nyns>
- Documentation source: <https://github.com/eotter-beep/nynsdocs>

You can improve NYNS itself, add new commands, or expand these docs with more
examples and guides. Pull requests are welcome.
