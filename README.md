import React, { useState } from "react";

export default function MockTrustWalletDemo() {
  const [selectedCoin, setSelectedCoin] = useState(null);
  const [showSend, setShowSend] = useState(false);
  const coins = [
    {
      id: "btc",
      name: "Bitcoin",
      symbol: "BTC",
      balance: 0.0,
      priceUsd: 54000.12,
      address: "1MockBTCAddrExmpl1234567890",
      color: "bg-yellow-400",
      txs: [
        { id: "t1", type: "received", amount: 0.0, date: "2025-09-12" },
        { id: "t2", type: "sent", amount: 0.0, date: "2025-08-05" }
      ]
    },
    {
      id: "eth",
      name: "Ethereum",
      symbol: "ETH",
      balance: 0.0,
      priceUsd: 3400.5,
      address: "0xMockETHAddressExample000000000000",
      color: "bg-purple-500",
      txs: [
        { id: "e1", type: "received", amount: 0.0, date: "2025-09-20" },
        { id: "e2", type: "sent", amount: 0.0, date: "2025-07-03" }
      ]
    },
    {
      id: "bnb",
      name: "BNB",
      symbol: "BNB",
      balance: 0.0,
      priceUsd: 300.0,
      address: "bnb1mockaddressexample00000000000",
      color: "bg-indigo-400",
      txs: [
        { id: "b1", type: "received", amount: 0.0, date: "2025-06-10" }
      ]
    }
  ];

  const formatUsd = (value) => {
    const v = (value || 0).toLocaleString(undefined, { minimumFractionDigits: 2, maximumFractionDigits: 2 });
    return `$${v}`;
  };

  const openCoin = (coin) => {
    setSelectedCoin(coin);
    setShowSend(false);
  };

  return (
    <div className="min-h-screen bg-gradient-to-br from-slate-100 to-white p-6">
      <div className="max-w-4xl mx-auto shadow-2xl rounded-2xl overflow-hidden grid grid-cols-1 md:grid-cols-3">
        <aside className="md:col-span-1 bg-white p-5">
          <div className="flex items-center gap-3">
            <div className="w-12 h-12 rounded-lg bg-gradient-to-tr from-blue-500 to-indigo-600 flex items-center justify-center text-white font-bold">WT</div>
            <div>
              <h3 className="text-lg font-semibold">Mock Wallet</h3>
              <p className="text-sm text-gray-500">Mock Wallet UI — addresses are placeholders</p>
            </div>
          </div>

          <div className="mt-6">
            <h4 className="text-sm text-gray-500">Total Balance</h4>
            <div className="mt-2 text-2xl font-bold">{formatUsd(coins.reduce((s, c) => s + Number(c.balance) * Number(c.priceUsd), 0))}</div>
          </div>

          <div className="mt-6">
            <h4 className="text-sm text-gray-500">Assets</h4>
            <ul className="mt-3 space-y-3">
              {coins.map((c) => (
                <li key={c.id} className="flex items-center justify-between p-3 rounded-xl border hover:shadow-sm cursor-pointer" onClick={() => openCoin(c)}>
                  <div className="flex items-center gap-3">
                    <div className={`w-10 h-10 rounded-lg flex items-center justify-center text-white ${c.color}`}>{c.symbol}</div>
                    <div>
                      <div className="text-sm font-medium">{c.name}</div>
                      <div className="text-xs text-gray-500">{c.balance} {c.symbol}</div>
                    </div>
                  </div>
                  <div className="text-sm text-gray-600">{formatUsd(Number(c.balance) * Number(c.priceUsd))}</div>
                </li>
              ))}
            </ul>
          </div>

          <div className="mt-6 text-center">
            <button className="px-4 py-2 rounded-lg bg-gradient-to-r from-green-400 to-teal-500 text-white font-semibold" onClick={() => { setSelectedCoin(null); setShowSend(false); }}>
              Overview
            </button>
          </div>
        </aside>

        <main className="md:col-span-2 bg-gradient-to-b from-white to-slate-50 p-6">
          {!selectedCoin && (
            <div>
              <h2 className="text-xl font-bold">Welcome to your Mock Wallet</h2>
              <p className="text-sm mt-2 text-gray-600">Click any asset on the left to view details. All addresses shown are placeholders.</p>

              <div className="mt-6 grid grid-cols-1 sm:grid-cols-2 gap-4">
                {coins.map((c) => (
                  <div key={c.id} className="p-4 rounded-xl border bg-white">
                    <div className="flex items-center justify-between">
                      <div className="flex items-center gap-3">
                        <div className={`w-11 h-11 rounded-lg flex items-center justify-center text-white ${c.color}`}>{c.symbol}</div>
                        <div>
                          <div className="font-semibold">{c.name}</div>
                          <div className="text-sm text-gray-500">{c.balance} {c.symbol}</div>
                        </div>
                      </div>
                      <div className="text-sm">{formatUsd(Number(c.balance) * Number(c.priceUsd))}</div>
                    </div>
                    <div className="mt-3 flex gap-2">
                      <button className="flex-1 px-3 py-2 rounded-lg border text-sm" onClick={() => openCoin(c)}>View</button>
                      <button className="px-3 py-2 rounded-lg bg-blue-600 text-white text-sm" onClick={() => { openCoin(c); setShowSend(true); }}>Send</button>
                    </div>
                  </div>
                ))}
              </div>
            </div>
          )}

          {selectedCoin && (
            <div>
              <div className="flex items-start justify-between">
                <div>
                  <div className="flex items-center gap-4">
                    <div className={`w-14 h-14 rounded-xl flex items-center justify-center text-white ${selectedCoin.color} font-bold text-lg`}>{selectedCoin.symbol}</div>
                    <div>
                      <div className="text-2xl font-bold">{selectedCoin.balance} {selectedCoin.symbol}</div>
                      <div className="text-sm text-gray-500">≈ {formatUsd(Number(selectedCoin.balance) * Number(selectedCoin.priceUsd))}</div>
                    </div>
                  </div>

                  <div className="mt-4 p-4 bg-white rounded-xl border">
                    <div className="text-xs text-gray-500">Address</div>
                    <div className="mt-2 flex items-center gap-3">
                      <div className="truncate font-mono text-sm">{selectedCoin.address}</div>
                      <button
                        className="px-3 py-1 rounded bg-slate-100 text-sm"
                        onClick={async () => {
                          try {
                            if (navigator.clipboard && navigator.clipboard.writeText) {
                              await navigator.clipboard.writeText(selectedCoin.address);
                              alert('Address copied to clipboard');
                            } else {
                              const ta = document.createElement('textarea');
                              ta.value = selectedCoin.address;
                              document.body.appendChild(ta);
                              ta.select();
                              document.execCommand('copy');
                              ta.remove();
                              alert('Address copied to clipboard');
                            }
                          } catch (e) {
                            alert('Copy failed');
                          }
                        }}
                      >
                        Copy
                      </button>
                      <button className="px-3 py-1 rounded bg-slate-100 text-sm" onClick={() => alert('No real action performed.')}>QR</button>
                    </div>
                    <p className="text-xs text-gray-400 mt-2">Note: This address is a placeholder and for UI purposes only.</p>
                  </div>
                </div>

                <div className="w-40">
                  <div className="flex flex-col gap-2">
                    <button className="px-3 py-2 rounded-lg bg-blue-600 text-white" onClick={() => setShowSend(true)}>Send</button>
                    <button className="px-3 py-2 rounded-lg border" onClick={() => alert('Receive action (demo)')}>Receive</button>
                    <button className="px-3 py-2 rounded-lg border" onClick={() => alert('Swap action (demo)')}>Swap</button>
                  </div>
                </div>
              </div>

              <div className="mt-6">
                <h4 className="text-sm text-gray-500">Recent Activity</h4>
                <ul className="mt-3 space-y-2">
                  {(selectedCoin.txs || []).map((t) => (
                    <li key={t.id} className="p-3 rounded-lg bg-white border flex items-center justify-between">
                      <div>
                        <div className="font-medium text-sm">{t.type === 'received' ? 'Received' : 'Sent'}</div>
                        <div className="text-xs text-gray-500">{t.date}</div>
                      </div>
                      <div className="font-semibold">{t.amount} {selectedCoin.symbol}</div>
                    </li>
                  ))}
                </ul>
              </div>
            </div>
          )}

          {showSend && selectedCoin && (
            <div className="fixed inset-0 flex items-center justify-center bg-black/40 p-4">
              <div className="w-full max-w-md bg-white rounded-2xl p-6">
                <h3 className="text-lg font-bold">Send {selectedCoin.symbol}</h3>
                <div className="mt-4 space-y-3">
                  <div>
                    <label className="block text-xs text-gray-500">To (paste address)</label>
                    <input className="w-full mt-1 p-2 border rounded" placeholder="e.g. 0xAddressExample..." />
                  </div>
                  <div>
                    <label className="block text-xs text-gray-500">Amount</label>
                    <input className="w-full mt-1 p-2 border rounded" placeholder={`Max ${selectedCoin.balance} ${selectedCoin.symbol}`} />
                  </div>
                  <div className="flex items-center gap-2">
                    <button className="px-4 py-2 bg-green-500 rounded text-white" onClick={() => alert('No real transaction created.')}>Confirm</button>
                    <button className="px-4 py-2 border rounded" onClick={() => setShowSend(false)}>Cancel</button>
                  </div>
                </div>
              </div>
            </div>
          )}

        </main>
      </div>

      <footer className="max-w-4xl mx-auto mt-6 text-center text-xs text-gray-500">This mock wallet is UI-only. It does NOT store private keys or interact with blockchains.</footer>
    </div>
  );
}
