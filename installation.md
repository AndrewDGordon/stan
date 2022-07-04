# How to install httpstan on Ubuntu in WSL2 on Windows 11

## Set up WSL

According to [Install Linux on Windows with WSL](https://docs.microsoft.com/en-us/windows/wsl/install), you can now install everything you need to run Windows Subsystem for Linux (WSL) by entering this command in an administrator PowerShell or Windows Command Prompt and then restarting your machine.
>wsl --install

You get the Ubuntu distribution by default.
It will prompt you to create a default account and password.

Scott Hanselman has a great YouTube video [WSL2, Visual Studio Code, Windows 10, Ubuntu/Linux + more](https://www.youtube.com/watch?v=Owrk9UxnMdI) worth watching to get familiar with WSL and also the Windows Terminal system for managing shells.

Enter all the commands below into a Ubuntu shell.

## View your Ubuntu directory using Windows explorer

>explorer.exe .

The Ubuntu file system is exposed to Windows, and you can use Windows Explorer to view and edit it.

## Set up C++ compiler

>sudo apt install gcc

>gcc --version

That returns: "gcc (Ubuntu 9.4.0-1ubuntu1~20.04.1) 9.4.0"

(Stan could also use clang but I couldn't get it to install.)

## Set up Python3

Ubuntu comes with python3, but not pip.

Download get-pip.py from the link on this [Stackoverflow page](https://stackoverflow.com/questions/4750806/how-do-i-install-pip-on-windows).

Then install pip by saying:
>sudo python get-pip.py

Then this magic is needed as per [Stackoverflow](https://stackoverflow.com/questions/21530577/fatal-error-python-h-no-such-file-or-directory).
>sudo apt-get install python3-dev

## Set up httpstan
First, off I tried installing prebuilt:
>python3 -m pip install httpstan

That did not work properly. TODO: check if the **magic** above fixed the problem without needing to compile from source.

So instead I followed the instructions on this [Stan Discourse page](https://discourse.mc-stan.org/t/httpstan-in-wsl2-ubuntu/23339/2) to compile from source.

## Testing httpstan

Follow the instructions on the [httpstan](https://httpstan.readthedocs.io/en/latest/).

These were the commands I used.

Start the service in one shell.
>python3 -m httpstan

Start another shell and test the service with these commands:

>curl -H "Content-Type: application/json" --data '{"program_code":"parameters {real y;} model {y ~ normal(0,1);}"}' http://localhost:8080/v1/models

You have to edit the encoded identifiers below based on the answer from the first command. Beware that this command returns lots of compiler warnings, which appear to be safe to ignore.

>curl -H "Content-Type: application/json" --data '{"function":"stan::services::sample::hmc_nuts_diag_e_adapt"}' http://localhost:8080/v1/models/3u47k7lm/fits

>curl http://localhost:8080/v1/models/3u47k7lm/fits/3uqfegtd > myfit.jsonlines

Go back to the first shell and stop the service with Control-C.

That's it.