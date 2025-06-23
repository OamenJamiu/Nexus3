# Nexus3
Final nexus Testnet before mainnet

**Requirements**

CPU = 2 - 4 cpu/vcpu

RAM = 4GB

STORAGE = 50GB TO 100GB SSD/NVMe


**VPS Only**
## --> Create account
* Create an account at https://app.nexus.xyz.


---

### 1. Install Dependecies
```bash
sudo apt update & sudo apt upgrade -y
```
```bash
sudo apt install screen curl build-essential pkg-config libssl-dev git-all -y
```
```bash
sudo apt install protobuf-compiler -y
```
```bash
sudo apt update
```
```bash
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```
Click Enter to continue
```bash
source $HOME/.cargo/env
```
```bash
rustup target add riscv32i-unknown-none-elf
```


### 2. Run Prover
**1- Start a screen to keep it running in the background**
```bash
screen -S nexus
```
**2- Install and run prover**
```bash
curl https://cli.nexus.xyz/ | sh
```
Tap on Y and Enter

**3- Run with an existing node ID**

```bash
source ~/.bashrc
```
```bash
nexus-network start --node-id your-node-id
```
* Replace `your-node-id` with the one acquired from nexus dashboard

---


## Debugging
### Error: nexus-network: /lib/x86_64-linux-gnu/libc.so.6: version `GLIBC_2.39' not found (required by nexus-network)
1- Confirm your installed GLIBC version:
```
ldd --version
```
* It must be `2.39`, if not, continue the steps.

2- Install dependencies:
```bash
sudo apt update
```
```bash
sudo apt install -y gawk bison gcc make wget tar
```

3- Download GLIBC 2.39:
```
wget -c https://ftp.gnu.org/gnu/glibc/glibc-2.39.tar.gz
```
```bash
tar -zxvf glibc-2.39.tar.gz
```
```bash
cd glibc-2.39
```

4- Create a build directory:
```
mkdir glibc-build
```
```bash
cd glibc-build
```

5- Build
```
../configure --prefix=/opt/glibc-2.39
```
```bash
make -j$(nproc)
```
```bash
sudo make install
```
```
cd
```

6- Instead of running `nexus-network` command, run `/opt/glibc-2.39/lib/ld-linux-x86-64.so.2 --library-path /opt/glibc-2.39/lib:/lib/x86_64-linux-gnu:/usr/lib/x86_64-linux-gnu /usr/local/bin/nexus-network`. For example: 
```
/opt/glibc-2.39/lib/ld-linux-x86-64.so.2 --library-path /opt/glibc-2.39/lib:/lib/x86_64-linux-gnu:/usr/lib/x86_64-linux-gnu /root/.nexus/bin/nexus-network register-user --wallet-address your-wallet-address
```
*  Note, In the above command, my `nexus-network` directory is `/root/.nexus/bin/nexus-network`, if you got error in finding the correct nexus directory, run these:
```
source ~/.bashrc
which nexus-network
```
* Now replace `/root/.nexus/bin/nexus-network` with the output.

Instead of running Step 3 command again, run this instead

```bash
/opt/glibc-2.39/lib/ld-linux-x86-64.so.2 --library-path /opt/glibc-2.39/lib:/lib/x86_64-linux-gnu:/usr/lib/x86_64-linux-gnu /root/.nexus/bin/nexus-network start --node-id your-node-id
```
* Replace `your-node-id` with the one acquired from nexus dashboard

---
