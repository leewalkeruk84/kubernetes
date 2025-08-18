# Kubectl Autocompete

## prerequisite

Ensure that the bash autocomplete package is installed
```bash
sudo apt-get install bash-completion
```

## automplete
Setup autocomplete in current bash shell
```bash
source <(kubectl completion bash)
```

Add autocomplete permanently to bash
```bash
echo "source <(kubectl completion bash)" >> ~/.bashrc
```