<script>
    let amount = 1;
    let from = 'KRW';
    let to = 'PHP';
    let conversion_rate = 0;
    let converted_amount = 0;
    let currencies = [];
  
    import { onMount } from 'svelte';
  
    onMount(async () => {
      const response = await fetch('https://api.exchangerate-api.com/v4/latest/KRW');
      const data = await response.json();
      currencies = Object.keys(data.rates);
      conversion_rate = data.rates[to];
      converted_amount = amount * conversion_rate;
    });
  
    const convertCurrency = async () => {
      const response = await fetch(`https://api.exchangerate-api.com/v4/latest/${from}`);
      const data = await response.json();
      conversion_rate = data.rates[to];
      converted_amount = amount * conversion_rate;
    };
  </script>
  
  <style>
    main {
      text-align: center;
      font-family: 'Arial', sans-serif;
      padding: 2rem;
      background-color: #f4f4f9;
      border-radius: 10px;
      max-width: 400px;
      margin: auto;
      box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
      margin-top: 100px;
    }
  
    h1 {
      color: #333;
      font-size: 2rem;
      margin-bottom: 1.5rem;
    }
  
    div {
      display: flex;
      justify-content: center;
      align-items: center;
      margin-bottom: 2rem;
    }
  
    input,
    select {
      padding: 0.8rem;
      font-size: 1rem;
      margin: 0.5rem;
      border: 2px solid #ccc;
      border-radius: 5px;
      transition: border-color 0.3s ease;
    }
  
    input:focus,
    select:focus {
      border-color: #007BFF;
      outline: none;
    }
  
    span {
      font-size: 1.2rem;
      font-weight: bold;
      margin: 0 1rem;
    }
  
    h2 {
      color: #333;
      font-size: 1.5rem;
      font-weight: 600;
      margin-top: 1.5rem;
    }
  
    .currency-converter-container {
      display: flex;
      flex-direction: column;
      align-items: center;
    }
  
    footer {
      font-size: 0.9rem;
      color: #888;
      margin-top: 2rem;
    }
  </style>
  
  <main>
    <h1>Currency Converter</h1>
  
    <div class="currency-converter-container">
      <input type="number" bind:value={amount} on:input={convertCurrency} min="0" />
      <div>
        <select bind:value={from} on:change={convertCurrency}>
          {#each currencies as currency}
            <option value={currency}>{currency}</option>
          {/each}
        </select>
        <span>to</span>
        <select bind:value={to} on:change={convertCurrency}>
          {#each currencies as currency}
            <option value={currency}>{currency}</option>
          {/each}
        </select>
      </div>
    </div>
  
    <h2>Converted Amount: {converted_amount.toFixed(2)} {to}</h2>
    <footer>
      <p>Created by Paulo Castro</p>
    </footer>
  </main>
  