import subprocess
import sys

def run_command(command):
    """Print the command, run it, and print results."""
    print(f"\n$ {command}\n")
    result = subprocess.run(command, shell=True, text=True, capture_output=True)

    print(result.stdout)
    if result.stderr:
        print("ERROR:", result.stderr)

    return result

def main():
    commit_message = "Auto commit"
    force = False

    args = sys.argv[1:]

    # Get commit message (-m "message")
    if "-m" in args:
        m_index = args.index("-m")
        if m_index + 1 < len(args):
            commit_message = args[m_index + 1]

    # Check for force flag (-f)
    if "-f" in args:
        force = True

    # Show git status
    print("\ngit status:")
    run_command("git status")

    # Commands to execute
    commands = [
        "git add .",
        f'git commit -m "{commit_message}"',
        "git push"
    ]

    # Print queued commands
    print("\nQueued commands:")
    for cmd in commands:
        print(cmd)

    # Confirmation step
    if not force:
        confirm = input("\nRun these commands? (y/n): ").lower()
        if confirm != "y":
            print("Cancelled.")
            return

    # Execute commands
    for cmd in commands:
        run_command(cmd)

if __name__ == "__main__":
    main()
