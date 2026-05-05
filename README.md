import React, { useState, useEffect } from 'react';
import { ethers } from 'ethers';

const ArcDexDemo = () => {
  const [amount, setAmount] = useState('');
  const [account, setAccount] = useState(null);
  const [exchangeRate] = useState(0.5); // Example: 1 ARC = 0.5 ETH

  const connectWallet = async () => {
    if (window.ethereum) {
      const accounts = await window.ethereum.request({ method: 'eth_requestAccounts' });
      setAccount(accounts[0]);
    } else {
      alert("Please install an Arc-compatible wallet or MetaMask!");
    }
  };

  const handleSwap = () => {
    console.log(`Swapping ${amount} tokens...`);
    alert(`Swap Successful: ${amount} ARC swapped for ${(amount * exchangeRate).toFixed(4)} ETH`);
  };

  return (
    <div style={styles.container}>
      <header style={styles.header}>
        <h2>Arc Demo DEX</h2>
        <button onClick={connectWallet} style={styles.button}>
          {account ? `${account.substring(0, 6)}...${account.substring(38)}` : "Connect Wallet"}
        </button>
      </header>

      <main style={styles.card}>
        <div style={styles.inputGroup}>
          <label>From (ARC)</label>
          <input 
            type="number" 
            placeholder="0.0" 
            value={amount}
            onChange={(e) => setAmount(e.target.value)}
            style={styles.input}
          />
        </div>

        <div style={styles.divider}>↓</div>

        <div style={styles.inputGroup}>
          <label>To (ETH)</label>
          <input 
            type="text" 
            readOnly 
            value={(amount * exchangeRate).toFixed(4)}
            style={styles.input}
          />
        </div>

        <button 
          onClick={handleSwap} 
          disabled={!amount || !account} 
          style={{...styles.swapButton, opacity: (!amount || !account) ? 0.5 : 1}}
        >
          Swap Tokens
        </button>
      </main>
    </div>
  );
};

const styles = {
  container: { fontFamily: 'Arial', padding: '20px', backgroundColor: '#0f172a', height: '100vh', color: 'white' },
  header: { display: 'flex', justifyContent: 'space-between', alignItems: 'center' },
  card: { background: '#1e293b', padding: '24px', borderRadius: '16px', maxWidth: '400px', margin: '40px auto' },
  inputGroup: { marginBottom: '15px' },
  input: { width: '100%', padding: '12px', borderRadius: '8px', border: 'none', marginTop: '8px', fontSize: '18px' },
  button: { padding: '10px 20px', borderRadius: '20px', cursor: 'pointer', border: 'none', background: '#38bdf8' },
  swapButton: { width: '100%', padding: '15px', borderRadius: '12px', background: '#38bdf8', color: 'white', fontWeight: 'bold', border: 'none', marginTop: '20px', cursor: 'pointer' },
  divider: { textAlign: 'center', margin: '10px 0', fontSize: '24px' }
};

export default ArcDexDemo;
