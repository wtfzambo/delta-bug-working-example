# Goal

The purpose of this repo is to help in reproduce a strange behavior with Delta Tables 1.0.0.

Package versions:

```bash
python = 3.7
delta = 1.0.0
spark = 3.1.2
java = openjdk 11.0.12
```

## Getting Started

### Installing Java

You need Java installed and JAVA_HOME set. If you don't have it, I recommend using SDKMAN since it's fast and easy.

```bash
curl -s "https://get.sdkman.io" | bash
# Replace `bash` with your own shell
```

Follow the instructions on-screen to complete installation. Then run:

```bash
source "$HOME/.sdkman/bin/sdkman-init.sh"
```

Ensure that the installation completed successfully:

```bash
sdk version
```

Finally, install java by running:

```bash
sdk install java 11.0.20-amzn
```

This should set `JAVA_HOME` by itself, but if it doesn't, run:

```bash
sdk use java 11.0.20-amzn
```

### Installing dependencies and setting SPARK_HOME

```bash
mkdir delta-test-bug && cd delta-test-bug
git clone https://github.com/wtfzambo/delta-bug-working-example.git .
```

The project is configured to run with python>=3.7 <3.8. If you don't have a compatible python version, install and set it for local use with:

```bash
pyenv install 3.7
pyenv local 3.7
poetry env use $(pyenv which python)
```

Run the following commands to complete the setup and set SPARK_HOME.

```bash
poetry install --no-root
poetry shell
```

Followed by:

```bash
sparkhome=$(echo 'sc.getConf.get("spark.home")' \
    | spark-shell \
    | grep "res0" \
    | cut -d\  -f4
) > /dev/null 2>&1
export SPARK_HOME=$sparkhome
```

Lastly, open the notebook server:

```bash
jupyter notebook
```

You should be good to go.
