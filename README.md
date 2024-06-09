
### کانفیگ مورد نیاز:
- Minimum: 2CPU, 4RAM, 30GB SSD
- Recommended: 8CPU, 12RAM, 100GB SSD

حتما اوبونتو 24 یا 22 نصب کنید ********************

### شروع کدها:

#### گام 1:
```
sudo apt-get update && apt-get upgrade -y
sudo apt install wget curl
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs/ | sh
apt install rustup
```




#### گام 2:
```
curl https://install.fuel.network | sh
```
در اینجا Y را تایپ کرده و سپس Enter را بزنید.

#### گام 3:
```
source /root/.bashrc
fuelup toolchain install latest
fuelup --version
```

#### گام 4:
```
fuelup self update
```

#### گام 5:
```
fuelup toolchain install nightly
```

#### گام 6:
```
fuelup default nightly
```

#### گام 7:
```
fuelup show
```

#### گام 8:
```
forc wallet new
```

در اینجا باید پسوردی برای خودتان انتخاب کنید، هنگام تایپ پسورد نمایش داده نمی‌شود. سپس Enter بزنید، دوباره پسورد را وارد کنید و Enter بزنید. بعد از آن کلمات بازیابی به شما داده می‌شود که باید ذخیره کنید.

#### گام 9:
```
forc wallet account new
```

در اینجا به شما آدرس ولت داده می‌شود که از سایت مربوطه (https://faucet-testnet.fuel.network/?__cf_chl_tk=A4rz_LuN.TzVmEXlzu7LZOcbpZYkRG08Iaosm73.eMA-1717140821-0.0.1.1-3775) باید به آدرس ولتتان فاست بزنید.

#### گام 10:
```
fuelup default
```

حالا به سایت [Alchemy](https://alchemy.com) رفته، اکانت بسازید، سپس در قسمت Apps گزینه Create new app را بزنید. در قسمت Chain، Ethereum و در قسمت Network، Sepolia را انتخاب کنید. نام و توضیحات وارد کنید و در نهایت روی Create app بزنید. حالا از قسمت API Key سه بخش API Key, HTTPS, WebSocket را ذخیره کنید.

#### گام 11:
```
git clone https://github.com/fmsuicmc/metadata-fuel
```

#### گام 12:
```
fuelup toolchain install testnet
```

#### گام 13:
```
fuelup default testnet
fuel-core-keygen new --key-type peering
```

حالا در این بخش p2p key را کپی کرده و در جای امنی ذخیره کنید.

#### گام 14:
```
apt install screen
screen -S fuel
```


#### گام 15:
در کد پایین، بخش ANY_SERVICE_NAME را با نام دلخواه پر کنید، بخش P2P_SECRET را با p2p key مربوطه و بخش ETH_RPC_ENDPOINT را با Rpc بخش HTTPS در سایت Alchemy قرار دهید.


نکته : اگر میخواید با نود Ritual ران کنید و تداخل پورت نداشته باشید اون Port رو بکنید  4050 
```
fuel-core run \
--service-name {ANY_SERVICE_NAME} \
--keypair {P2P_SECRET} \
--relayer {ETH_RPC_ENDPOINT} \
--ip 0.0.0.0 --port 4000 --peering-port 30333 \
--db-path ~/.testnet \
--snapshot $Home/root/metadata-fuel \
--utxo-validation --poa-instant false --enable-p2p \
--min-gas-price 1 --max-block-size 18874368 --max-transmit-size 18874368 \
--reserved-nodes /dns4/p2p-devnet.fuel.network/tcp/30333/p2p/16Uiu2HAm6pmJUedRFjennk4A8yWL6zCApHCuykzRRroqMjjxZ8o6,/dns4/p2p-devnet.fuel.network/tcp/30334/p2p/16Uiu2HAm8dBwTRzqazCMqQDdR8thMa7BKiW4ep2B4DoQQp6Qhyfd \
--sync-header-batch-size 100 \
--enable-relayer \
--relayer-v2-listening-contracts 0x01855B78C1f8868DE70e84507ec735983bf262dA \
--relayer-da-deploy-height 5827607 \
--relayer-log-page-size 2000
```

در نهایت Enter بزنید و با دستور Ctrl+A+D از صفحه خارج شوید.

به سایت Alchemy بازگردید و در قسمت Apps روی اپی که ساختید کلیک کنید. اگر اعداد صفر نباشند یعنی درست انجام داده‌اید.


----------------------------------------------------------

ساخت اپلیکیشن و دیپلوی کانترکت

# ساخت پروژه
```
mkdir fuel-project
cd fuel-project
forc new counter-contract
```
# تغییر کانترکت
```
nano counter-contract/src/main.sw
```


# Delete everything and Paste this code
```
contract;
 
storage {
    counter: u64 = 0,
}
 
abi Counter {
    #[storage(read, write)]
    fn increment();
 
    #[storage(read)]
    fn count() -> u64;
}
 
impl Counter for Contract {
    #[storage(read)]
    fn count() -> u64 {
        storage.counter.read()
    }
 
    #[storage(read, write)]
    fn increment() {
        let incremented = storage.counter.read() + 1;
        storage.counter.write(incremented);
    }
}
```
press crtl + x
press y
press enter

```
cd counter-contract
forc build

```


# Deploy Contract , Enter password , Enter 1 , Enter y
```
forc deploy --testnet
```

# You get a Contract ID in your Terminal like this, Save it!
Contract counter-contract Deployed!

Network: https://testnet.fuel.network
Contract ID: 0x8342d413de2a678245d9ee39f020795800c7e6a4ac5ff7daae275f533dc05e08
Deployed in block 0x4ea52b6652836c499e44b7e42f7c22d1ed1f03cf90a1d94cd0113b9023dfa636


# Check Nodejs Version
```
node --version
```
# Delete old files
```
sudo apt-get remove nodejs
sudo apt-get purge nodejs
sudo apt-get autoremove
sudo rm /etc/apt/keyrings/nodesource.gpg
sudo rm /etc/apt/sources.list.d/nodesource.list
```

# Install Nodejs 18
```
NODE_MAJOR=18
sudo apt-get update
sudo apt-get install -y ca-certificates curl gnupg
sudo mkdir -p /etc/apt/keyrings
```
```
curl -fsSL https://deb.nodesource.com/gpgkey/nodesource-repo.gpg.key | sudo gpg --dearmor -o /etc/apt/keyrings/nodesource.gpg
```
```
echo "deb [signed-by=/etc/apt/keyrings/nodesource.gpg] https://deb.nodesource.com/node_${NODE_MAJOR}.x nodistro main" | sudo tee /etc/apt/sources.list.d/nodesource.list
```
```
sudo apt-get update
sudo apt-get install -y nodejs
node --version
```

```
cd $HOME && cd fuel-project
npx create-react-app frontend --template typescript
```
# Success! Created frontend at Fuel/fuel-project/frontend

# Install fuels sdk
```
ls
cd frontend
npm install fuels @fuels/react @fuels/connectors @tanstack/react-query
```
```
npm audit fix --force
```

# Generate Contract type
```
npx fuels init --contracts ../counter-contract/ --output ./src/sway-api
npx fuels build
```

# Building..
Building Sway programs using source 'forc' binary
Generating types..
🎉  Build completed successfully!

# If you get Config file not found again do this:
```
npx fuels init --contracts ../counter-contract/ --output ./src/sway-api
```

# Then
```
npx fuels build
```


# Edit index
```
nano src/index.tsx
```

# Delete everything and Paste this code
```
import React from 'react';
import ReactDOM from 'react-dom/client';
import './index.css';
import App from './App';
import reportWebVitals from './reportWebVitals';
import { FuelProvider } from '@fuels/react';
import {
  defaultConnectors,
} from '@fuels/connectors';
import { QueryClient, QueryClientProvider } from '@tanstack/react-query';

const queryClient = new QueryClient();

const root = ReactDOM.createRoot(
  document.getElementById('root') as HTMLElement
);
root.render(
  <React.StrictMode>
    <QueryClientProvider client={queryClient}>
      <FuelProvider
        fuelConfig={{
          connectors: defaultConnectors(),
        }}
      >
        <App />
      </FuelProvider>
    </QueryClientProvider>
  </React.StrictMode>
);

// If you want to start measuring performance in your app, pass a function
// to log results (for example: reportWebVitals(console.log))
// or send to an analytics endpoint. Learn more: https://bit.ly/CRA-vitals
reportWebVitals();
```


# Edit App
```
nano src/App.tsx
```

# Delete everything and Paste this code
# Replace Contract ID

```
import { useEffect, useState } from "react";
import {
  useBalance,
  useConnectUI,
  useIsConnected,
  useWallet
} from '@fuels/react';
import { CounterContractAbi__factory  } from "./sway-api"
import type { CounterContractAbi } from "./sway-api";
 
// REPLACE WITH YOUR CONTRACT ID
const CONTRACT_ID =
  "0x6a99c924a15e3f9c35a759e4534f04560263b2cf512ad7998ab5194c487c3a4c";
 
export default function Home() {
  const [contract, setContract] = useState<CounterContractAbi>();
  const [counter, setCounter] = useState<number>();
  const { connect, isConnecting } = useConnectUI();
  const { isConnected } = useIsConnected();
  const { wallet } = useWallet();
  const { balance } = useBalance({
    address: wallet?.address.toAddress(),
    assetId: wallet?.provider.getBaseAssetId(),
  });
 
  useEffect(() => {
    async function getInitialCount(){
      if(isConnected && wallet){
        const counterContract = CounterContractAbi__factory.connect(CONTRACT_ID, wallet);
        await getCount(counterContract);
        setContract(counterContract);
      }
    }
    
    getInitialCount();
  }, [isConnected, wallet]);
 
  const getCount = async (counterContract: CounterContractAbi) => {
    try{
      const { value } = await counterContract.functions
      .count()
      .get();
      setCounter(value.toNumber());
    } catch(error) {
      console.error(error);
    }
  }
 
  const onIncrementPressed = async () => {
    if (!contract) {
      return alert("Contract not loaded");
    }
    try {
      await contract.functions
      .increment()
      .call();
      await getCount(contract);
    } catch(error) {
      console.error(error);
    }
  };
 
  return (
    <div style={styles.root}>
      <div style={styles.container}>
        {isConnected ? (
          <>
            <h3 style={styles.label}>Counter</h3>
            <div style={styles.counter}>
              {counter ?? 0}
            </div>
 
            {balance && balance.toNumber() === 0 ? (
              <p>Get testnet funds from the <a target="_blank" rel="noopener noreferrer"  href={`https://faucet-testnet.fuel.network/?address=${wallet?.address.toAddress()}`}>Fuel Faucet</a> to increment the counter.</p>
          ) : 
          (
            <button
            onClick={onIncrementPressed}
            style={styles.button}
            >
              Increment Counter
            </button>
          )
          }
            
          <p>Your Fuel Wallet address is:</p>
          <p>{wallet?.address.toAddress()}</p>
          </>
        ) : (
          <button
          onClick={() => {
            connect();
          }}
          style={styles.button}
          >
            {isConnecting ? 'Connecting' : 'Connect'}
          </button>
        )}
      </div>
    </div>
  );
}
 
const styles = {
  root: {
    display: 'grid',
    placeItems: 'center',
    height: '100vh',
    width: '100vw',
    backgroundColor: "black",
  } as React.CSSProperties,
  container: {
    color: "#ffffffec",
    display: "flex",
    flexDirection: "column",
    alignItems: "center",
  } as React.CSSProperties,
  label: {
    fontSize: "28px",
  },
  counter: {
    color: "#a0a0a0",
    fontSize: "48px",
  },
  button: {
    borderRadius: "8px",
    margin: "24px 0px",
    backgroundColor: "#707070",
    fontSize: "16px",
    color: "#ffffffec",
    border: "none",
    outline: "none",
    height: "60px",
    padding: "0 1rem",
    cursor: "pointer"
  },
}
```

# Start Dapp
```
npm audit fix --force
```
```
npm start
```

Compiled successfully!

You can now view frontend in the browser.

  Local:            http://localhost:3000
  On Your Network:  http://192.168.4.48:3000

Note that the development build is not optimized.
To create a production build, use npm run build.

# It can be 3001 for you in your browser, you have to check
# Open port (Don't touch this terminal and create a new terminal)
ufw allow 3000/tcp
