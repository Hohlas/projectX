## Project X Sol Relayer

[projectx.run](https://projectx.run) | [Install Relayer](https://docs.projectx.run/how-to-connect/install-relayer#begin-with-neccesary-software-install)

<details>
<summary>neccesary software install</summary> 
  
```bash  
sudo apt update && sudo apt upgrade -y
sudo apt install libssl-dev libudev-dev pkg-config zlib1g-dev llvm clang cmake make libprotobuf-dev protobuf-compiler -y
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh -s -- -y
. "$HOME/.cargo/env"            # For sh/bash/zsh/ash/dash/pdksh
# source $HOME/.cargo/env
```
</details>



<details>
<summary>Clone relayer repo and build binary</summary> 

### clone & buid  

```bash  
cd $HOME
if [ -d ~/lite-relayer ]; then rm -r ~/lite-relayer; fi
git clone https://github.com/projectxsol/lite-relayer.git
cd lite-relayer
git fetch
git submodule update --init --recursive &&
cargo build --release --bin transaction-relayer
cd ~/lite-relayer/target/release
RELAYER_TAG=$(./transaction-relayer -V | awk '{print $2}') # check version
zip ~/projectx_relayer.zip transaction-relayer
```

### push bin 2 git

```bash  
cd $HOME
if [ -d ~/lite-relayer ]; then rm -r ~/lite-relayer; fi
git clone https://github.com/Hohlas/projectX.git ~/lite-relayer
rm -r ~/lite-relayer/target/release/*
cd ~/lite-relayer/target/release
git config --global user.email "mail@hohla.ru"
git config --global user.name "Hohlas"
mv ~/projectx_relayer.zip ~/lite-relayer/target/release/projectx_relayer.zip
echo "projectX relayer $RELAYER_TAG" > README.md
git add .
git commit -m "Add projectx_relayer.zip v$RELAYER_TAG"
git push https://$PAT@github.com/Hohlas/projectX.git main
```

</details>



### copy bin from this git

```bash
cd $HOME
if [ -d ~/lite-relayer ]; then rm -r ~/lite-relayer; fi
git clone https://github.com/Hohlas/projectX.git ~/lite-relayer
cp ~/lite-relayer/relayer.service ~/solana/relayer.service
ln -sfn ~/solana/relayer.service /etc/systemd/system # projectx-relayer.service
unzip -oj ~/lite-relayer/target/release/projectx_relayer.zip -d ~/lite-relayer/target/release
~/lite-relayer/target/release/transaction-relayer -V
```

```bash
# switch to local relayer
solana-validator -l $HOME/solana/ledger set-relayer-config --relayer-url http://127.0.0.1:11226 
```
```bash
# switch to public relayer
solana-validator -l ~/solana/ledger set-relayer-config --relayer-url http://frankfurt.mainnet.relayer.jito.wtf:8100 
```
```bash
# switch block-engine
solana-validator -l $HOME/solana/ledger set-block-engine-config --block-engine-url https://frankfurt.mainnet.block-engine.jito.wtf
```






