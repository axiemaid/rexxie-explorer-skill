# Rexxie Explorer API

Base URL: `https://rexxie.axiemaid.com`

## Endpoints

### GET /collection
Collection metadata (name, total, deploy block, etc.)

### GET /nfts?page=1&limit=50
Paginated NFT list. Each NFT: `{ number, mintTxid, image, traits, owner, burned, transfers[] }`

### GET /nft/:number
Single NFT by number (1-2222).

### GET /nft/tx/:txid
Lookup NFT by mint transaction ID.

### GET /owner/:address
All NFTs owned by a BSV address. Returns `{ address, count, nfts[] }`.

### GET /traits
Trait distribution by type. Returns `{ background: { Purple: 386, ... }, body: { ... }, ... }`

### GET /traits/:type/:value
NFTs with a specific trait. Returns `{ trait, type, value, count, nfts[] }`.

### GET /search?q=query
Search by number, owner address, or trait value.

### GET /random
Random NFT.

### GET /stats
Collection stats: total, indexed, unique owners, burned count, trait counts.

### GET /health
Health check.

## Image URLs
NFTs have `image` (remote tique.run URL). Local images served at `https://rexxie.axiemaid.com/images/{number}.png`.
