![Ichivaults Logo](https://ichi.org/static/img_logo_ichi_black-1c38d8d8e04542b1421ef1a1b2169f57.svg)
# @ichidao/ichi-vaults-sdk
![MIT License](https://badgen.net/badge/license/MIT/blue)

This sdk contains collection of functions to interact with IchiVault's smart contract.

## Table of Contents

* [__Installation__](#Installation)
* [__Usage__](#Usage)
     * [__Vault Functions__](#Vault)
        * [`approveDepositToken()`](#1-approveDepositToken)
        * [`deposit()`](#2-depositLP)
        * [`depositNativeToken()`](#3-depositLP)
        * [`approveVaultToken()`](#4-approveDepositToken)
        * [`isVaultTokenApproved()`](#5-approveDepositToken)
        * [`withdraw()`](#6-withdraw)
        * [`withdrawWithSlippage()`](#7-withdraw)
        * [`withdrawNativeToken()`](#8-withdraw)
        * [`isDepositTokenApproved()`](#9-isDepositTokenApproved)
        * [`isTokenAllowed()`](#10-isTokenAllowed)
        * [`getMaxDepositAmount()`](#11-getMaxDepositAmount)
        * [`getUserBalance()`](#12-getUserBalance)
        * [`getUserAmounts()`](#13-getUserAmounts)
        * [`getTotalSupply()`](#14-getTotalSupply)
        * [`getTotalAmounts()`](#15-getTotalAmounts)
        * [`getFeesCollected()`](#16-getFeesCollected)
        * [`getFeesCollectedInfo()`](#17-getFeesCollectedInfo)
        * [`getAverageDepositTokenRatios()`](#18-getAverageDepositTokenRatios)
        * [`getLpApr()`](#19-getLpApr)
        * [`getLpPriceChange()`](#20-getLpPriceChange)
        * [`getIchiVaultInfo()`](#21-getIchiVaultInfo)
        * [`getVaultsByTokens()`](#22-getVaultsByTokens)

## Installation
Install with
```bash
yarn add @ichidao/ichi-vaults-sdk
```
or
```bash
npm install @ichidao/ichi-vaults-sdk
```

## Usage
### Vault

#### 1. `approveDepositToken()`

| param | type |  default | required
| -------- | -------- | -------- | --------
| accountAddress   | string | - | true
| tokenIdx           | 0 \| 1 | - | true
| vaultAddress   | string | - | true |
| jsonProvider      | [JsonRpcProvider](https://github.com/ethers-io/ethers.js/blob/f97b92bbb1bde22fcc44100af78d7f31602863ab/packages/providers/src.ts/json-rpc-provider.ts#L393) | - | true
| dex   | SupportedDex | - | true |
| amount   | string \| number | undefined | false |
| overrides         | [Overrides](https://github.com/ethers-io/ethers.js/blob/f97b92bbb1bde22fcc44100af78d7f31602863ab/packages/contracts/lib/index.d.ts#L7)  | undefined | false

<br/>

```typescript
import { Web3Provider } from '@ethersproject/providers';
import { approveDepositToken, SupportedDex } from '@ichidao/ichi-vaults-sdk';

const web3Provider = new Web3Provider(YOUR_WEB3_PROVIDER);
const vaultAddress = "0x3ac9...a5f132"
const accountAddress = "0xaaaa...aaaaaa"
const amount = 100
const dex = SupportedDex.UniswapV3

const txnDetails = await approveDepositToken(
    accountAddress,
    0, // token idx can be 0 or 1
    vaultAddress,
    web3Provider,
    dex,
    amount // (optional)
);

await txnDetails.wait();

// can now deposit token0
// ...
```

#### 2. `deposit()`

| param | type |  default | required
| -------- | -------- | -------- | --------
| accountAddress   | string | - | true
| amount0           | string \| number | - | true
| amount1           | string \| number | - | true
| vaultAddress   | string | - | true
| jsonProvider      | [JsonRpcProvider](https://github.com/ethers-io/ethers.js/blob/f97b92bbb1bde22fcc44100af78d7f31602863ab/packages/providers/src.ts/json-rpc-provider.ts#L393) | - | true
| dex   | SupportedDex | - | true
| percentSlippage   | number | 1 | false
| overrides         | [Overrides](https://github.com/ethers-io/ethers.js/blob/f97b92bbb1bde22fcc44100af78d7f31602863ab/packages/contracts/lib/index.d.ts#L7)  | undefined | false

<br/>

```typescript
import { Web3Provider } from '@ethersproject/providers';
import { deposit, SupportedDex } from '@ichidao/ichi-vaults-sdk';

const web3Provider = new Web3Provider(YOUR_WEB3_PROVIDER);
const vaultAddress = "0x3ac9...a5f132"
const dex = SupportedDex.UniswapV3
const accountAddress = "0xaaaa...aaaaaa"

const amount0 = 100
const amount1 = 0

const txnDetails = await deposit(
    accountAddress,
    amount0, // can be 0 when only depositing amount1
    amount1, // can be 0 when only depositing amount0
    vaultAddress,
    web3Provider,
    dex,
    1 // acceptable slippage (percents)
)
```

#### 3. `depositNativeToken()`

| param | type |  default | required
| -------- | -------- | -------- | --------
| accountAddress   | string | - | true
| amount0           | string \| number | - | true
| amount1           | string \| number | - | true
| vaultAddress   | string | - | true
| jsonProvider      | [JsonRpcProvider](https://github.com/ethers-io/ethers.js/blob/f97b92bbb1bde22fcc44100af78d7f31602863ab/packages/providers/src.ts/json-rpc-provider.ts#L393) | - | true
| dex   | SupportedDex | - | true
| percentSlippage   | number | 1 | false
| overrides         | [Overrides](https://github.com/ethers-io/ethers.js/blob/f97b92bbb1bde22fcc44100af78d7f31602863ab/packages/contracts/lib/index.d.ts#L7)  | undefined | false

<br/>

```typescript
import { Web3Provider } from '@ethersproject/providers';
import { deposit, SupportedDex } from '@ichidao/ichi-vaults-sdk';

const web3Provider = new Web3Provider(YOUR_WEB3_PROVIDER);
const vaultAddress = "0x3ac9...a5f132"
const dex = SupportedDex.UniswapV3
const accountAddress = "0xaaaa...aaaaaa"

const amount0 = 100
const amount1 = 0

const txnDetails = await depositNativeToken(
    accountAddress,
    amount0, // can be 0 when only depositing amount1
    amount1, // can be 0 when only depositing amount0
    vaultAddress,
    web3Provider,
    dex,
    1 // acceptable slippage (percents)
)
```

#### 4. `approveVaultToken()`

| param | type |  default | required
| -------- | -------- | -------- | --------
| accountAddress   | string | - | true
| vaultAddress   | string | - | true |
| jsonProvider      | [JsonRpcProvider](https://github.com/ethers-io/ethers.js/blob/f97b92bbb1bde22fcc44100af78d7f31602863ab/packages/providers/src.ts/json-rpc-provider.ts#L393) | - | true
| dex   | SupportedDex | - | true |
| shares   | string \| number | undefined | false |
| overrides         | [Overrides](https://github.com/ethers-io/ethers.js/blob/f97b92bbb1bde22fcc44100af78d7f31602863ab/packages/contracts/lib/index.d.ts#L7)  | undefined | false

<br/>

```typescript
import { Web3Provider } from '@ethersproject/providers';
import { approveDepositToken, SupportedDex } from '@ichidao/ichi-vaults-sdk';

const web3Provider = new Web3Provider(YOUR_WEB3_PROVIDER);
const vaultAddress = "0x3ac9...a5f132"
const accountAddress = "0xaaaa...aaaaaa"
const amount = 100
const dex = SupportedDex.UniswapV3

const txnDetails = await approveVaultToken(
    accountAddress,
    vaultAddress,
    web3Provider,
    dex,
    amount // (optional)
);

await txnDetails.wait();

// can now deposit token0
// ...
```

#### 5. `isVaultTokenApproved()`

| param | type |  default | required
| -------- | -------- | -------- | --------
| accountAddress   | string | - | true
| shares   | string \| number, | - | true |
| vaultAddress   | string | - | true |
| jsonProvider      | [JsonRpcProvider](https://github.com/ethers-io/ethers.js/blob/f97b92bbb1bde22fcc44100af78d7f31602863ab/packages/providers/src.ts/json-rpc-provider.ts#L393) | - | true
| dex   | SupportedDex | - | true |

<br/>

```typescript
import { Web3Provider } from '@ethersproject/providers';
import { isDepositTokenApproved, SupportedDex } from '@ichidao/ichi-vaults-sdk';

const web3Provider = new Web3Provider(YOUR_WEB3_PROVIDER);
const vaultAddress = "0x3ac9...a5f132"
const accountAddress = "0xaaaa...aaaaaa"
const amount = 100
const dex = SupportedDex.UniswapV3

const isToken0Approved: boolean = await isDepositTokenApproved(
    accountAddress,
    amount,
    vaultAddress,
    web3Provider,
    dex
)
```

#### 6. `withdraw()`

| param | type |  default | required
| -------- | -------- | -------- | --------
| accountAddress   | string | - | true
| shares           | string \| number | - | true
| vaultAddress   | string | - | true
| jsonProvider      | [JsonRpcProvider](https://github.com/ethers-io/ethers.js/blob/f97b92bbb1bde22fcc44100af78d7f31602863ab/packages/providers/src.ts/json-rpc-provider.ts#L393) | - | true
| dex   | SupportedDex | - | true
| overrides         | [Overrides](https://github.com/ethers-io/ethers.js/blob/f97b92bbb1bde22fcc44100af78d7f31602863ab/packages/contracts/lib/index.d.ts#L7)  | undefined | false

<br/>

```typescript
import { Web3Provider } from '@ethersproject/providers';
import { getUserBalance, withdraw, SupportedDex } from '@ichidao/ichi-vaults-sdk';

const web3Provider = new Web3Provider(YOUR_WEB3_PROVIDER);
const vaultAddress = "0x3ac9...a5f132"
const dex = SupportedDex.UniswapV3
const accountAddress = "0xaaaa...aaaaaa"

const totalUserShares: string = await getUserBalance(
    accountAddress,
    vaultAddress,
    web3Provider
    dex,
)

let shares = Number(totalUserShare) * 0.5 // 50% of user deshare balance

const txnDetails = await withdraw(
    accountAddress,
    shares,
    vaultAddress,
    web3Provider,
    dex
)
```

#### 7. `withdrawWithSlippage()`

| param | type |  default | required
| -------- | -------- | -------- | --------
| accountAddress   | string | - | true
| shares           | string \| number | - | true
| vaultAddress   | string | - | true
| jsonProvider      | [JsonRpcProvider](https://github.com/ethers-io/ethers.js/blob/f97b92bbb1bde22fcc44100af78d7f31602863ab/packages/providers/src.ts/json-rpc-provider.ts#L393) | - | true
| dex   | SupportedDex | - | true
| percentSlippage   | number | 1 | false
| overrides         | [Overrides](https://github.com/ethers-io/ethers.js/blob/f97b92bbb1bde22fcc44100af78d7f31602863ab/packages/contracts/lib/index.d.ts#L7)  | undefined | false

<br/>

```typescript
import { Web3Provider } from '@ethersproject/providers';
import { getUserBalance, withdraw, SupportedDex } from '@ichidao/ichi-vaults-sdk';

const web3Provider = new Web3Provider(YOUR_WEB3_PROVIDER);
const vaultAddress = "0x3ac9...a5f132"
const dex = SupportedDex.UniswapV3
const accountAddress = "0xaaaa...aaaaaa"

const totalUserShares: string = await getUserBalance(
    accountAddress,
    vaultAddress,
    web3Provider
    dex,
)

let shares = Number(totalUserShare) * 0.5 // 50% of user deshare balance

const txnDetails = await withdraw(
    accountAddress,
    shares,
    vaultAddress,
    web3Provider,
    dex
)
```

#### 8. `withdrawNativeToken()`

| param | type |  default | required
| -------- | -------- | -------- | --------
| accountAddress   | string | - | true
| shares           | string \| number | - | true
| vaultAddress   | string | - | true
| jsonProvider      | [JsonRpcProvider](https://github.com/ethers-io/ethers.js/blob/f97b92bbb1bde22fcc44100af78d7f31602863ab/packages/providers/src.ts/json-rpc-provider.ts#L393) | - | true
| dex   | SupportedDex | - | true
| percentSlippage   | number | 1 | false
| overrides         | [Overrides](https://github.com/ethers-io/ethers.js/blob/f97b92bbb1bde22fcc44100af78d7f31602863ab/packages/contracts/lib/index.d.ts#L7)  | undefined | false

<br/>

```typescript
import { Web3Provider } from '@ethersproject/providers';
import { getUserBalance, withdraw, SupportedDex } from '@ichidao/ichi-vaults-sdk';

const web3Provider = new Web3Provider(YOUR_WEB3_PROVIDER);
const vaultAddress = "0x3ac9...a5f132"
const dex = SupportedDex.UniswapV3
const accountAddress = "0xaaaa...aaaaaa"

const totalUserShares: string = await getUserBalance(
    accountAddress,
    vaultAddress,
    web3Provider
    dex,
)

let shares = Number(totalUserShare) * 0.5 // 50% of user deshare balance

const txnDetails = await withdraw(
    accountAddress,
    shares,
    vaultAddress,
    web3Provider,
    dex
)
```

#### 9. `isDepositTokenApproved()`

| param | type |  default | required
| -------- | -------- | -------- | --------
| accountAddress   | string | - | true
| tokenIdx           | 0 \| 1 | - | true
| amount   | string \| number, | - | true |
| vaultAddress   | string | - | true |
| jsonProvider      | [JsonRpcProvider](https://github.com/ethers-io/ethers.js/blob/f97b92bbb1bde22fcc44100af78d7f31602863ab/packages/providers/src.ts/json-rpc-provider.ts#L393) | - | true
| dex   | SupportedDex | - | true |

<br/>

```typescript
import { Web3Provider } from '@ethersproject/providers';
import { isDepositTokenApproved, SupportedDex } from '@ichidao/ichi-vaults-sdk';

const web3Provider = new Web3Provider(YOUR_WEB3_PROVIDER);
const vaultAddress = "0x3ac9...a5f132"
const accountAddress = "0xaaaa...aaaaaa"
const amount = 100
const dex = SupportedDex.UniswapV3

const isToken0Approved: boolean = await isDepositTokenApproved(
    accountAddress,
    0, // token idx can be 0 or 1
    amount,
    vaultAddress,
    web3Provider,
    dex
)
```

#### 10. `isTokenAllowed()`

| param | type |  default | required
| -------- | -------- | -------- | --------
| tokenIdx           | 0 \| 1 | - | true
| vaultAddress   | string | - | true |
| jsonProvider      | [JsonRpcProvider](https://github.com/ethers-io/ethers.js/blob/f97b92bbb1bde22fcc44100af78d7f31602863ab/packages/providers/src.ts/json-rpc-provider.ts#L393) | - | true
| dex   | SupportedDex | - | true |

<br/>

```typescript
import { Web3Provider } from '@ethersproject/providers';
import { isTokenAllowed, SupportedDex } from '@ichidao/ichi-vaults-sdk';

const web3Provider = new Web3Provider(YOUR_WEB3_PROVIDER);
const vaultAddress = "0x3ac9...a5f132"
const dex = SupportedDex.UniswapV3

const isAllowed = await isTokenAllowed(
    0, // token idx can be 0 or 1
    vaultAddress,
    web3Provider,
    dex
)

```

#### 11. `getMaxDepositAmount()`

| param | type |  default | required
| -------- | -------- | -------- | --------
| tokenIdx           | 0 \| 1 | - | true
| vaultAddress   | string | - | true |
| jsonProvider      | [JsonRpcProvider](https://github.com/ethers-io/ethers.js/blob/f97b92bbb1bde22fcc44100af78d7f31602863ab/packages/providers/src.ts/json-rpc-provider.ts#L393) | - | true
| dex   | SupportedDex | - | true |

<br/>

```typescript
import { Web3Provider } from '@ethersproject/providers';
import { getMaxDepositAmount, SupportedDex } from '@ichidao/ichi-vaults-sdk';

const web3Provider = new Web3Provider(YOUR_WEB3_PROVIDER);
const vaultAddress = "0x3ac9...a5f132"
const dex = SupportedDex.UniswapV3

const maxAmount = await getMaxDepositAmount(
    0, // token idx can be 0 or 1
    vaultAddress,
    web3Provider,
    dex
)

```

#### 12. `getUserBalance()`

| param | type |  default | required
| -------- | -------- | -------- | --------
| accountAddress   | string | - | true |
| vaultAddress   | string | - | true |
| jsonProvider      | [JsonRpcProvider](https://github.com/ethers-io/ethers.js/blob/f97b92bbb1bde22fcc44100af78d7f31602863ab/packages/providers/src.ts/json-rpc-provider.ts#L393) | - | true
| dex   | SupportedDex | - | true
| raw   | true | undefined | false |

<br/>

```typescript
import { Web3Provider } from '@ethersproject/providers';
import { getUserBalance, SupportedDex } from '@ichidao/ichi-vaults-sdk';

const web3Provider = new Web3Provider(YOUR_WEB3_PROVIDER);
const vaultAddress = "0x3ac9...a5f132"
const dex = SupportedDex.UniswapV3
const accountAddress = "0xaaaa...aaaaaa"

const shares: string = await getUserBalance(
    accountAddress,
    vaultAddress,
    web3Provider
    dex
)

// - or -

const sharesBN: BigNumber = await getUserBalance(
    accountAddress,
    vaultAddress,
    web3Provider
    dex,
    true
)
```

#### 13. `getUserAmounts()`

| param | type |  default | required
| -------- | -------- | -------- | --------
| accountAddress   | string | - | true |
| vaultAddress   | string | - | true |
| jsonProvider      | [JsonRpcProvider](https://github.com/ethers-io/ethers.js/blob/f97b92bbb1bde22fcc44100af78d7f31602863ab/packages/providers/src.ts/json-rpc-provider.ts#L393) | - | true
| dex   | SupportedDex | - | true
| raw   | true | undefined | false |

<br/>

```typescript
import { Web3Provider } from '@ethersproject/providers';
import { getUserAmounts, SupportedDex } from '@ichidao/ichi-vaults-sdk';

const web3Provider = new Web3Provider(YOUR_WEB3_PROVIDER);
const vaultAddress = "0x3ac9...a5f132"
const dex = SupportedDex.UniswapV3
const accountAddress = "0xaaaa...aaaaaa"

const amounts: [string, string] & {amount0: string, amount1: string} = await getUserAmounts(
    accountAddress,
    vaultAddress,
    web3Provider
    dex,
)

// - or -

const amountsBN: [BigNumber, BigNumber] & {amount0: BigNumber, amount1: BigNumber} = await getUserAmounts(
    accountAddress,
    vaultAddress,
    web3Provider
    dex,
    true
)
```

#### 14. `getTotalSupply()`

| param | type |  default | required
| -------- | -------- | -------- | --------
| vaultAddress   | string | - | true |
| jsonProvider      | [JsonRpcProvider](https://github.com/ethers-io/ethers.js/blob/f97b92bbb1bde22fcc44100af78d7f31602863ab/packages/providers/src.ts/json-rpc-provider.ts#L393) | - | true
| dex   | SupportedDex | - | true
| raw   | true | undefined | false |

<br/>

```typescript
import { Web3Provider } from '@ethersproject/providers';
import { getTotalSupply, SupportedDex } from '@ichidao/ichi-vaults-sdk';

const web3Provider = new Web3Provider(YOUR_WEB3_PROVIDER);
const vaultAddress = "0x3ac9...a5f132"
const dex = SupportedDex.UniswapV3

const shares: string = await getTotalSupply(
    accountAddress,
    vaultAddress,
    web3Provider
    dex,
)

// - or -

const sharesBN: BigNumber = await getTotalSupply(
    accountAddress,
    vaultAddress,
    web3Provider
    dex,
    true
)
```

#### 15. `getTotalAmounts()`

| param | type |  default | required
| -------- | -------- | -------- | --------
| vaultAddress   | string | - | true |
| jsonProvider      | [JsonRpcProvider](https://github.com/ethers-io/ethers.js/blob/f97b92bbb1bde22fcc44100af78d7f31602863ab/packages/providers/src.ts/json-rpc-provider.ts#L393) | - | true
| dex   | SupportedDex | - | true
| raw   | true | undefined | false |

<br/>

```typescript
import { Web3Provider } from '@ethersproject/providers';
import { getTotalAmounts, SupportedDex } from '@ichidao/ichi-vaults-sdk';

const web3Provider = new Web3Provider(YOUR_WEB3_PROVIDER);
const vaultAddress = "0x3ac9...a5f132"
const dex = SupportedDex.UniswapV3
const accountAddress = "0xaaaa...aaaaaa"

const amounts: [string, string] & {total0: string, total1: string} = await getTotalAmounts(
    accountAddress,
    vaultAddress,
    web3Provider
    dex,
)

// - or -

const amountsBN: [BigNumber, BigNumber] & {total0: BigNumber, total1: BigNumber} = await getTotalAmounts(
    accountAddress,
    vaultAddress,
    web3Provider
    dex,
    true
)
```

#### 16. `getFeesCollected()`

| param | type |  default | required
| -------- | -------- | -------- | --------
| vaultAddress   | string | - | true |
| jsonProvider      | [JsonRpcProvider](https://github.com/ethers-io/ethers.js/blob/f97b92bbb1bde22fcc44100af78d7f31602863ab/packages/providers/src.ts/json-rpc-provider.ts#L393) | - | true
| dex   | SupportedDex | - | true
| rawOrDays   | true or number | undefined | false |
| days   | number | undefined | false |

<br/>

```typescript
import { Web3Provider } from '@ethersproject/providers';
import { getFeesCollected, SupportedDex } from '@ichidao/ichi-vaults-sdk';

const web3Provider = new Web3Provider(YOUR_WEB3_PROVIDER);
const vaultAddress = "0x3ac9...a5f132";
const dex = SupportedDex.UniswapV3;
const days = 7;

const amounts: [string, string] & {total0: string, total1: string} = await getFeesCollected(
    vaultAddress,
    web3Provider
    dex,
)

// - or -

const amountsBN: [BigNumber, BigNumber] & {total0: BigNumber, total1: BigNumber} = await getFeesCollected(
    vaultAddress,
    web3Provider
    dex,
    true
)

// - or -

const amounts: [string, string] & {total0: string, total1: string} = await getFeesCollected(
    vaultAddress,
    web3Provider
    dex,
    days
)

// - or -

const amountsBN: [BigNumber, BigNumber] & {total0: BigNumber, total1: BigNumber} = await getFeesCollected(
    vaultAddress,
    web3Provider
    dex,
    true,
    days
)
```

#### 17. `getFeesCollectedInfo()`

| param | type |  default | required
| -------- | -------- | -------- | --------
| vaultAddress   | string | - | true |
| jsonProvider      | [JsonRpcProvider](https://github.com/ethers-io/ethers.js/blob/f97b92bbb1bde22fcc44100af78d7f31602863ab/packages/providers/src.ts/json-rpc-provider.ts#L393) | - | true
| dex   | SupportedDex | - | true
| forDays   | number[] | undefined | false |

<br/>
If forDays is not specified, returns FeesInfo for time periods of 1, 7, and 30 days.
<br/>

```typescript
import { Web3Provider } from '@ethersproject/providers';
import { getFeesCollectedInfo, SupportedDex } from '@ichidao/ichi-vaults-sdk';

const web3Provider = new Web3Provider(YOUR_WEB3_PROVIDER);
const vaultAddress = "0x3ac9...a5f132";
const dex = SupportedDex.UniswapV3;
const days = [2, 5, 14, 60];

const feesInfo: FeesInfo[] = await getFeesCollectedInfo(
    vaultAddress,
    web3Provider
    dex,
)

// - or -

const feesInfo: FeesInfo[] = await getFeesCollectedInfo(
    vaultAddress,
    web3Provider
    dex,
    days,
)
```

#### 18. `getAverageDepositTokenRatios()`

| param | type |  default | required
| -------- | -------- | -------- | --------
| vaultAddress   | string | - | true |
| jsonProvider      | [JsonRpcProvider](https://github.com/ethers-io/ethers.js/blob/f97b92bbb1bde22fcc44100af78d7f31602863ab/packages/providers/src.ts/json-rpc-provider.ts#L393) | - | true
| dex   | SupportedDex | - | true
| timeIntervals   | number[] | [1, 7, 30] | false |

<br/>

```typescript
import { Web3Provider } from '@ethersproject/providers';
import { getFeesCollectedInfo, SupportedDex } from '@ichidao/ichi-vaults-sdk';

const web3Provider = new Web3Provider(YOUR_WEB3_PROVIDER);
const vaultAddress = "0x3ac9...a5f132";
const dex = SupportedDex.UniswapV3;
const days = [2, 5, 14, 60];

const averageDtr: AverageDepositTokenRatio[] = await getAverageDepositTokenRatios(
    vaultAddress,
    web3Provider
    dex,
)

// - or -

const averageDtr: AverageDepositTokenRatio[] = await getAverageDepositTokenRatios(
    vaultAddress,
    web3Provider
    dex,
    days,
)
```

#### 19. `getLpApr()`

| param | type |  default | required
| -------- | -------- | -------- | --------
| vaultAddress   | string | - | true |
| jsonProvider      | [JsonRpcProvider](https://github.com/ethers-io/ethers.js/blob/f97b92bbb1bde22fcc44100af78d7f31602863ab/packages/providers/src.ts/json-rpc-provider.ts#L393) | - | true
| dex   | SupportedDex | - | true
| timeIntervals   | number[] | [1, 7, 30] | false |

<br/>

```typescript
import { Web3Provider } from '@ethersproject/providers';
import { getLpApr, SupportedDex, VaultApr } from '@ichidao/ichi-vaults-sdk';

const web3Provider = new Web3Provider(YOUR_WEB3_PROVIDER);
const vaultAddress = "0x3ac9...a5f132";
const dex = SupportedDex.UniswapV3;
const days = [2, 5, 14, 60];

const averageDtr: VaultApr[] = await getLpApr(
    vaultAddress,
    web3Provider
    dex,
)

// - or -

const averageDtr: VaultApr[] = await getLpApr(
    vaultAddress,
    web3Provider
    dex,
    days,
)
```

#### 20. `getAverageDepositTokenRatios()`

| param | type |  default | required
| -------- | -------- | -------- | --------
| vaultAddress   | string | - | true |
| jsonProvider      | [JsonRpcProvider](https://github.com/ethers-io/ethers.js/blob/f97b92bbb1bde22fcc44100af78d7f31602863ab/packages/providers/src.ts/json-rpc-provider.ts#L393) | - | true
| dex   | SupportedDex | - | true
| timeIntervals   | number[] | [1, 7, 30] | false |

<br/>

```typescript
import { Web3Provider } from '@ethersproject/providers';
import { getFeesCollectedInfo, SupportedDex } from '@ichidao/ichi-vaults-sdk';

const web3Provider = new Web3Provider(YOUR_WEB3_PROVIDER);
const vaultAddress = "0x3ac9...a5f132";
const dex = SupportedDex.UniswapV3;
const days = [2, 5, 14, 60];

const averageDtr: AverageDepositTokenRatio[] = await getAverageDepositTokenRatios(
    vaultAddress,
    web3Provider
    dex,
)

// - or -

const averageDtr: AverageDepositTokenRatio[] = await getAverageDepositTokenRatios(
    vaultAddress,
    web3Provider
    dex,
    days,
)
```

#### 21. `getIchiVaultInfo()`

| param | type |  default | required
| -------- | -------- | -------- | --------
| chain      | SupportedChain | - | true
| dex   | SupportedDex | - | true
| vaultAddress   | string | - | true |
| jsonProvider   | JsonRpcProvider | - | false |

<br/>

```typescript
import { Web3Provider } from '@ethersproject/providers';
import { getIchiVaultInfo, SupportedDex, SupportedChain, IchiVault } from '@ichidao/ichi-vaults-sdk';

const vaultAddress = "0x3ac9...a5f132"
const dex = SupportedDex.UniswapV3;
const chain = SupportedChain.Polygon;

const vaultInfo = await getIchiVaultInfo(chain, dex, vaultAddress);
if (vaultInfo) {
    const addressTokenA = vaultInfo.tokenA;
}
```

#### 22. `getVaultsByTokens()`

| param | type |  default | required
| -------- | -------- | -------- | --------
| chain      | SupportedChain | - | true
| dex   | SupportedDex | - | true
| depositTokenAddress   | string | - | true |
| pairedTokenAddress   | string | - | true |

<br/>

```typescript
import { Web3Provider } from '@ethersproject/providers';
import { getVaultsByTokens, SupportedDex, SupportedChain, IchiVault } from '@ichidao/ichi-vaults-sdk';

const depositToken = "0x1b...bfd6"
const pairedToken = "0x11...c4d6"
const dex = SupportedDex.UniswapV3;
const chain = SupportedChain.Polygon;

const vault = await getVaultsByTokens(chain, dex, depositToken, pairedToken)
if (!vault) {
    console.log("Couldn't find vaults with these parameters")
} else {
    const vaultAddress = vault.id;
}

```
## Types

### SupportedChainId

```typescript
enum SupportedChainId {
  arbitrum = 42161,
  base = 8453,
  blast = 81457,
  blast_sepolia_testnet = 168587773,
  bsc = 56,
  celo = 42220,
  eon = 7332,
  evmos = 9001,
  fantom = 250,
  hedera_testnet = 296,
  linea = 59144,
  mainnet = 1,
  mantle = 5000,
  polygon = 137,
  polygon_zkevm = 1101,
  zksync_era = 324,
  zksync_era_testnet = 280,
}
```

### SupportedDex

```typescript
enum SupportedDex {
  Ascent = 'Ascent',
  Blueprint = 'Blueprint',
  Cleo = 'Cleo',
  Equalizer = 'Equalizer',
  Fenix = 'Fenix',
  Forge = 'Forge',
  Horiza = 'Horiza',
  Lynex = 'Lynex',
  Pancakeswap = 'PancakeSwap',
  Quickswap = 'QuickSwap',
  Ramses = 'Ramses',
  Retro = 'Retro',
  SaucerSwap = 'SaucerSwap',
  Sushiswap = 'SushiSwap',
  Thena = 'Thena',
  UniswapV3 = 'Uniswap V3',
  Velocore = 'Velocore',
}
```

### IchiVault

```typescript
interface IchiVault {
  id: string; // vault address
  tokenA: string; // token0 address
  tokenB: string; // token1 address
  allowTokenA: boolean;
  allowTokenB: boolean;
}
```

### FeesInfo

```typescript
type FeesInfo  = {
  timePeriod: number; // in days
  feeAmount0: string; // in token0
  feeAmount1: string; // in token1
  pctAPR: number; // yearly APR based on the timePeriod
}
```

### AverageDepositTokenRatio

```typescript
type AverageDepositTokenRatio  = {
  timePeriod: number; // in days
  percent: number;
}
```

### VaultApr

```typescript
type VaultApr  = {
  timeInterval: number; // in days
  apr: number; // percent
}
```

### PriceChange

```typescript
type PriceChange  = {
  timeInterval: number; // in days
  priceChange: number; 
}
```
