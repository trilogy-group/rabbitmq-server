FROM ubuntu:18.04

RUN apt-get update && \
    apt-get upgrade -y && \
    apt-get install locales -y && \
    locale-gen en_US en_US.UTF-8 && \
    update-locale LANG=en_US.UTF-8 && \
    echo 'export LANG=en_US.UTF-8\nexport LC_ALL=en_US.UTF-8' >> ~/.bashrc

RUN apt-get install vim wget telnet iputils-ping -y && \
    apt-get clean

RUN apt-get -y install git make python gnupg gnupg1 gnupg2 curl rsync zip

# Erlang and Elixir 1.7.3.1
RUN mkdir -p /installers && \
    cd /installers && \
    wget https://packages.erlang-solutions.com/erlang-solutions_1.0_all.deb && \
    dpkg -i erlang-solutions_1.0_all.deb && \
    apt-get update && \
    apt-get install esl-erlang -y && \
    apt-get install elixir -y && \
    rm -f erlang-solutions_1.0_all.deb
    
WORKDIR /data

EXPOSE 5672

CMD tail -f /dev/null
