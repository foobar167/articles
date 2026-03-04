- [Task](#task)
- [Links](#links)
- [Install KoboldCpp on Ubuntu](#install)
- [Useful information](#useful_info)

---
### <a name="task" />Task

Install and configure KoboldCpp.

KoboldCpp is an easy-to-use AI text-generation software
for GGML and GGUF models, inspired by the original KoboldAI.

---
### <a name="links" />Links

* The KoboldCpp FAQ and Knowledgebase: https://github.com/LostRuins/koboldcpp/wiki
* [Latest releases](https://github.com/LostRuins/koboldcpp/releases). File `koboldcpp-linux-x64` for Ubuntu
* Download models from https://huggingface.co/koboldcpp
* [KoboldCpp Quick Launch Templates for newbies](https://huggingface.co/koboldcpp/newbie-templates/blob/main/README.md)
* For questions come to the discord at https://koboldai.org/discord

---
### <a name="install" />Install KoboldCpp on Ubuntu

```shell
cd ~/Downloads/
curl -fLo koboldcpp https://github.com/LostRuins/koboldcpp/releases/latest/download/koboldcpp-linux-x64 && chmod +x koboldcpp
chmod +x koboldcpp-linux-x64  # make executable file
./koboldcpp-linux-x64 --version  # get version
./koboldcpp-linux-x64 --help  # get help
# Load settings from a .kcpps file. Other arguments will be ignored
./koboldcpp-linux-x64 --config LowSpec-Everything.kcppt

# URL: http://localhost:5001
# API: http://localhost:5001/api
# Image generator: http://localhost:5001/sdui
```

---
### <a name="useful_info" />Useful information

Useful keys from the help. Check out some `*.kcppt` files to configure usage:
   * `--password [API key]`  Enter a password required to use this instance.
This key will be required for all text endpoints.
**WARNING: Image endpoints are not secured**.
   * `--remotetunnel`  Uses Cloudflare to create a remote tunnel,
allowing you to access `koboldcpp` remotely over the internet
even behind a firewall.
   * `--sdclipgpu`  Put CLIP and T5 to GPU for image generation.
Otherwise, CLIP will use CPU.
   * `--sdlora [filename] [[filename] ...]`  Specify image generation LoRAs
safetensors models to be applied. Multiple LoRAs are accepted.

[For better security](https://github.com/LostRuins/koboldcpp/wiki#how-can-i-better-secure-a-public-koboldcpp-instance)
do not use `--admin` on a public instance, or ensure it's used with a strong password.
Set a `--password` to prevent unauthorized generations.
