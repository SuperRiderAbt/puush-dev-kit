# Puush Developer Kit

This kit provides the **minimal ABIs, contract addresses, and tools** needed to build bots and services around the Puush platform.  
It is designed to expose only the **essential functions and events**, while keeping internal logic private.

---

## 🌐 Network

- **Chain**: Cronos Mainnet  
- **Chain ID**: `25`  
- **Public RPC**:  
  ```txt
  https://evm.cronos.org
  ```

---

## 🏗 Main Contract

- **Name**: `PuushdotFun`  
- **Purpose**: Factory / Router  
- **Address**:  
  ```txt
  0x0F73F73707C0f365808b1C50a0187E17E7F897c9
  ```

---

## 💧 Funds Contracts

- **Name**: `PuushdotFunds`  
- **Purpose**: Bonding curve liquidity (holds CRO + tokens)  
- **Note**: Each created coin has its **own Funds contract**  
- **Lookup**: Use  
  ```solidity
  fundsMap(coinAddress)
  ```  
  on the main contract to find the matching Funds contract.

---

## 📡 Main Contract Events

- **CoinCreated** → A new token is launched  
- **CoinsBought / CoinsSold** → Trading activity for a coin  
- **GraduatedAndBurned** → Coin leaves its bonding curve (graduation event)

---

## ⚙️ Funds Contract Functions

- `getFunds()` → Get current reserves (tokens + CRO)  
- `getAmountToBuy()` / `getCost()` → Simulate a buy  
- `getReturn()` → Simulate a sell  

---

## 📦 Version

`v1.0.4`

---

## ⚠️ Disclaimer

These ABIs and addresses are provided for **developer convenience** only.  

Bots and integrations must handle:
- Chain reorganizations (reorgs)  
- Failed or reverted transactions  
- Possible future contract upgrades  

This kit does **not** expose the full contract logic by design.
