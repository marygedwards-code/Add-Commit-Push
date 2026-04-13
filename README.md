import subprocess
import sys

def run_command(command):
    """Print command, run it, then print results."""
    print(f"\n$ {command}\n")
    result = subprocess.run(command, shell=True, text=True, capture_output=True)
    print(result.stdout)
    if result.stderr:
        print("ERROR:", result.stderr)
    return result

def main():
    commit_message = "Auto commit"
    force = False

    # Parse arguments
    args = sys.argv[1:]

    if "-m" in args:
        idx = args.index("-m")
        if idx + 1 < len(args):
            commit_message = args[idx + 1]

    if "-f" in args:
        force = True

    # Step 1: git status
    print("\ngit status:")
    run_command("git status")

    # Commands to run
    commands = [
        "git add .",
        f'git commit -m "{commit_message}"',
        "git push"
    ]

    print("\nQueued Commands:")
    for c in commands:
        print(c)

    # Confirmation
    if not force:
        confirm = input("\nRun these commands? (y/n): ").lower()
        if confirm != "y":
            print("Cancelled.")
            return

    # Execute commands
    for c in commands:
        run_command(c)

if __name__ == "__main__":
    main()# Add-Commit-Push
