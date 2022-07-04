On Ubuntu in wsl2

sudo apt install gcc

Download get-pip.py from https://stackoverflow.com/questions/4750806/how-do-i-install-pip-on-windows

sudo python get-pip.py

python3 -m pip install httpstan

python3 -m httpstan
Starts service, but it won't respond to requests.
Problem seems to be gcc failing.

curl -H "Content-Type: application/json" \
ata '{">     --data '{"program_code":"parameters {real y;} model {y ~ normal(0,1);}"}' \
>     http://localhost:8080/v1/models

curl -H "Content-Type: application/json" \
    --data '{"function":"stan::services::sample::hmc_nuts_diag_e_adapt"}' \
    http://localhost:8080/v1/models/3u47k7lm/fits

curl http://localhost:8080/v1/models/3u47k7lm/fits/eqtkydnj > myfit.jsonlines

https://discourse.mc-stan.org/t/httpstan-in-wsl2-ubuntu/23339/2

https://stackoverflow.com/questions/21530577/fatal-error-python-h-no-such-file-or-directory
fix:
sudo apt-get install python3-dev
