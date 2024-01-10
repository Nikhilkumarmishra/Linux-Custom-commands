Attached all the screenshots needed

# SECTION A
![Screenshot (370)](https://github.com/Nikhilkumarmishra/Linux-Custom-commands/assets/87891556/9512f1c5-bbda-4d09-a7d0-8809385eee75)

#!/bin/bash

# here i am Defining the version
VERSION="v0.1.0"

# here i am definig  the Function to display the manual page
display_manual() {
  echo "internsctl(1) - Custom Linux Command"
  echo
  echo "NAME"
  echo "    internsctl - Perform operations as per user's requirements"
  echo
  echo "SYNOPSIS"
  echo "    internsctl [OPTIONS] [ARGUMENTS]"
  echo
  echo "DESCRIPTION"
  echo "    internsctl is a custom Linux command that performs operations as specified by the user."
  echo
  echo "OPTIONS"
  echo "    --help      Display this help message"
  echo "    --version   Display the version of internsctl"
  echo
  echo "EXAMPLES"
  echo "    internsctl --help"
  echo "    internsctl --version"
  echo
}

# Function to display help message
display_help() {
  echo "Usage: internsctl [OPTIONS] [ARGUMENTS]"
  echo "Perform operations as per user's requirements."
  echo
  echo "OPTIONS:"
  echo "  --help      Display this help message"
  echo "  --version   Display the version of internsctl"
  echo
  echo "EXAMPLES:"
  echo "  internsctl --help"
  echo "  internsctl --version"
  echo
}

# Function to display version
display_version() {
  echo "internsctl $VERSION"
}

# Main script logic
case "$1" in
  --help)
    display_help
    ;;
  --version)
    display_version
    ;;
  *)
    echo "Unknown option. Use 'internsctl --help' for usage information."
    exit 1
    ;;
esac


# SECTION B

![Screenshot (371)](https://github.com/Nikhilkumarmishra/Linux-Custom-commands/assets/87891556/e3a25c31-0ed0-40fc-9163-0861d7900aef)

# Create a script file, let's call it internsctl, and make it executable:
touch internsctl
chmod +x internsctl

#!/bin/bash

case "$1" in
    "cpu")
        case "$2" in
            "getinfo")
                lscpu
                ;;
            *)
                echo "Invalid subcommand for 'cpu'. Usage: internsctl cpu getinfo"
                ;;
        esac
        ;;
    "memory")
        case "$2" in
            "getinfo")
                free -h
                ;;
            *)
                echo "Invalid subcommand for 'memory'. Usage: internsctl memory getinfo"
                ;;
        esac
        ;;
    *)
        echo "Invalid command. Usage: internsctl {cpu|memory} {getinfo}"
        ;;
esac

# Save the file and run your commands:
./internsctl cpu getinfo
./internsctl memory getinfo

# These commands will provide output similar to lscpu and free -h, respectively




# part 2 |  Intermediate
![Screenshot (372)](https://github.com/Nikhilkumarmishra/Linux-Custom-commands/assets/87891556/fdedcf3d-5e59-449b-a49e-90438c825ada)

# Create a script file, let's call it internsctl, and make it executable:
touch internsctl
chmod +x internsctl

# Add the following
#!/bin/bash

case "$1" in
    "user")
        case "$2" in
            "create")
                if [ -z "$3" ]; then
                    echo "Error: Please provide a username."
                else
                    sudo useradd -m "$3"
                    echo "User $3 created successfully."
                fi
                ;;
            "list")
                if [ "$3" == "--sudo-only" ]; then
                    grep -E 'sudo|admin' /etc/group | cut -d: -f4 | tr ',' '\n'
                else
                    getent passwd | grep -E '/bin/(bash|sh)$' | cut -d: -f1
                fi
                ;;
            *)
                echo "Invalid subcommand for 'user'. Usage: internsctl user {create <username> | list [--sudo-only]}"
                ;;
        esac
        ;;
    *)
        echo "Invalid command. Usage: internsctl {user}"
        ;;
esac

# This script uses a case statement to handle different commands and subcommands for user management. It checks whether the first argument is "user" and then processes the provided subcommands accordingly.

# Save and run your command

./internsctl user create <username>
./internsctl user list
./internsctl user list --sudo-only

# These commands should create a user, list all regular users, and list users with sudo permissions, respectively.





# Part 3 | Advance Level 

![Screenshot (373)](https://github.com/Nikhilkumarmishra/Linux-Custom-commands/assets/87891556/f16ce1cf-c359-4948-aa46-61b9401c9c5a)

# You can extend the internsctl script to handle file information with options

#!/bin/bash

print_file_info() {
    local file=$1
    local size=$(stat -c %s "$file")
    local permissions=$(stat -c %A "$file")
    local owner=$(stat -c %U "$file")
    local last_modified=$(stat -c %y "$file")

    echo "File: $file"
    echo "Access: $permissions"
    echo "Size(B): $size"
    echo "Owner: $owner"
    echo "Modify: $last_modified"
}

while [[ "$#" -gt 0 ]]; do
    case "$1" in
        "file")
            shift
            case "$1" in
                "getinfo")
                    shift
                    file_name="$1"
                    shift
                    if [ ! -e "$file_name" ]; then
                        echo "Error: File '$file_name' does not exist."
                        exit 1
                    fi

                    if [ "$#" -eq 0 ]; then
                        print_file_info "$file_name"
                    else
                        while [[ "$#" -gt 0 ]]; do
                            case "$1" in
                                "--size" | "-s")
                                    stat -c %s "$file_name"
                                    ;;
                                "--permissions" | "-p")
                                    stat -c %A "$file_name"
                                    ;;
                                "--owner" | "-o")
                                    stat -c %U "$file_name"
                                    ;;
                                "--last-modified" | "-m")
                                    stat -c %y "$file_name"
                                    ;;
                                *)
                                    echo "Invalid option: $1"
                                    exit 1
                                    ;;
                            esac
                            shift
                        done
                    fi
                    ;;
                *)
                    echo "Invalid subcommand for 'file'. Usage: internsctl file getinfo [options] <file-name>"
                    exit 1
                    ;;
            esac
            ;;
        *)
            echo "Invalid command. Usage: internsctl {file}"
            exit 1
            ;;
    esac
    shift
done

# Now we can use the command as described:

# For general file information
./internsctl file getinfo hello.txt

# For specific information using options
./internsctl file getinfo --size hello.txt
./internsctl file getinfo --permissions hello.txt
./internsctl file getinfo --owner hello.txt
./internsctl file getinfo --last-modified hello.txt


![Screenshot (375)](https://github.com/Nikhilkumarmishra/Linux-Custom-commands/assets/87891556/1e074792-a56f-4ca2-9f59-20c0a3d96a66)



# In this file i have tried to add all the codes necessary for executing all the 3 task . and rest of details is attached in the Readme file .  Due to time contraint other wise i can elaborate it more with more detials. 

![Screenshot (374)](https://github.com/Nikhilkumarmishra/Linux-Custom-commands/assets/87891556/122614f5-31f4-4951-be0c-4afb313bb8e3)
![Screenshot (373)](https://github.com/Nikhilkumarmishra/Linux-Custom-commands/assets/87891556/f16ce1cf-c359-4948-aa46-61b9401c9c5a)
![Screenshot (372)](https://github.com/Nikhilkumarmishra/Linux-Custom-commands/assets/87891556/fdedcf3d-5e59-449b-a49e-90438c825ada)
![Screenshot (371)](https://github.com/Nikhilkumarmishra/Linux-Custom-commands/assets/87891556/e3a25c31-0ed0-40fc-9163-0861d7900aef)
![Screenshot (370)](https://github.com/Nikhilkumarmishra/Linux-Custom-commands/assets/87891556/9512f1c5-bbda-4d09-a7d0-8809385eee75)

Diagram , seprate folder for diagram is also there in the repository 
![image](https://github.com/Nikhilkumarmishra/Linux-Custom-commands/assets/87891556/bc42b806-e556-4931-baa9-024ccfbda6e6)
