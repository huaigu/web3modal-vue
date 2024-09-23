<script setup>
import { createAppKit, useAppKit, useWalletInfo,useAppKitState  } from '@reown/appkit/vue'

import { WagmiProvider } from 'wagmi'
import { arbitrum, mainnet } from '@reown/appkit/networks'
import { WagmiAdapter } from '@reown/appkit-adapter-wagmi'

import dotenv from 'dotenv'
dotenv.config()

// 1. Get projectId from https://cloud.reown.com
const projectId = process.env.PROJECT_ID

// 2. Create a metadata object - optional
const metadata = {
  name: 'AppKit',
  description: 'AppKit Example',
  url: 'https://web3modal.com', // origin must match your domain & subdomain
  icons: ['https://avatars.githubusercontent.com/u/179229932']
}

// 3. Create Wagmi Adapter
const wagmiAdapter = new WagmiAdapter({
  ssr: true,
  projectId,
  networks
})

// 4. Create modal
createAppKit({
  adapters: [wagmiAdapter],
  networks: [mainnet, arbitrum],
  metadata,
  projectId,
  features: {
    analytics: true // Optional - defaults to your Cloud configuration
  }
})

// 5. Use modal composable
const modal = useAppKit()

const { walletInfo } = useWalletInfo()
const { open, selectedNetworkId } = useAppKitState()
</script>

<script>
export const networks = [mainnet, arbitrum]
</script>

<style>
.container {
  border: 1px solid #ddd;
  background-color: #f5f5f5;
  padding: 20px;
}
</style>

<template>
  <div>
    <div class="container">
      <h2 style="text-align: left;">w3m-connect-button</h2>
      <div style="display: flex; align-items: center;">
        <p style="margin-right: 10px;">Size SM</p>
        <w3m-connect-button  size="sm" />
      </div>
      <div style="display: flex; align-items: center;">
        <p style="margin-right: 10px;">Size MD</p> <!-- 添加描述 -->
        <w3m-connect-button size="md" />
      </div>
      <div style="display: flex; align-items: center;">
        <p style="margin-right: 10px;">Custom text in label</p> <!-- 添加描述 -->
        <w3m-connect-button size="md" label="Connect Your Wallet" />
      </div>
      <div style="display: flex; align-items: center;">
        <p style="margin-right: 10px;">Custom text when modal is open</p> <!-- 添加描述 -->
        <w3m-connect-button size="md" loadingLabel="Loading..." />
      </div>
    </div> 
    
    <div class="container">
      <h2 style="text-align: left;">w3m-account-button</h2>
      <w3m-account-button balance="show"/>
    </div>

    <div class="container">
      <h2 style="text-align: left;">w3m-network-button</h2>
      <w3m-network-button/>
    </div>
    <!-- 添加结束标签 -->

    <div class="container">
      <h2 style="text-align: left;">Custom Button</h2>
      <button @click="modal.open()">Open Modal</button>
    </div>

    <div class="container">
      <h2 style="text-align: left;">Wallet Info</h2>
      <div style="display: flex; align-items: center;">
        <img :src="walletInfo?.icon" alt="Wallet Image" style="height: 1em; margin-right: 10px;" />
        <div style="display: flex; align-items: center;">
          <p style="text-align: left; margin-right: 10px;">Wallet Name: {{ walletInfo?.name }}</p>
        </div>
      </div>
    </div>
  </div>
</template>