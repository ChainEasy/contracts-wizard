<script lang="ts">
  import HelpTooltip from './HelpTooltip.svelte';

  import type { KindedOptions } from '@openzeppelin/wizard';
  import { erc20, premintPattern, infoDefaults } from '@openzeppelin/wizard';

  import AccessControlSection from './AccessControlSection.svelte';
  import UpgradeabilitySection from './UpgradeabilitySection.svelte';
  import InfoSection from './InfoSection.svelte';

  export const opts: Required<KindedOptions['ERC20']> = {
    kind: 'ERC20',
    ...erc20.defaults,
    premint: '', // default to empty premint in UI instead of 0
    info: { ...infoDefaults }, // create new object since Info is nested
  };

  $: requireAccessControl = erc20.isAccessControlRequired(opts);
</script>

<section class="controls-section">
  <h1>Settings</h1>

    <div class="grid grid-cols-[2fr,1fr] gap-2">
      <label class="labeled-input">
        <span>Token Name</span>
        <input bind:value={opts.name}>
      </label>

      <label class="labeled-input">
        <span>Ticker</span>
        <input bind:value={opts.symbol}>
      </label>
    </div>

    <label class="labeled-input">
      <span class="flex justify-between pr-2">
        Total Supply
        <HelpTooltip>Create an initial amount of tokens for the deployer.</HelpTooltip>
      </span>
      <input bind:value={opts.premint} placeholder="0" pattern={premintPattern.source}>
    </label>
</section>

<section class="controls-section">
  <h1>Features</h1>

  <div class="checkbox-group">
    <label class:checked={opts.mintable}>
      <input type="checkbox" bind:checked={opts.mintable}>
      Can Mint Tokens
      <HelpTooltip>
        Privileged accounts will be able to create more supply.
      </HelpTooltip>
    </label>

    <label class:checked={opts.burnable}>
      <input type="checkbox" bind:checked={opts.burnable}>
      Can Burn Tokens
      <HelpTooltip link="https://docs.openzeppelin.com/contracts/4.x/api/token/erc20#ERC20Burnable">
        Token holders will be able to destroy their tokens.
      </HelpTooltip>
    </label>

    <label class:checked={opts.pausable}>
      <input type="checkbox" bind:checked={opts.pausable}>
      Can Pause All TX
      <HelpTooltip link="https://docs.openzeppelin.com/contracts/4.x/api/security#Pausable">
        Privileged accounts will be able to pause the functionality marked as <code>whenNotPaused</code>.
        Useful for emergency response.
      </HelpTooltip>
    </label>

    <label class:checked={opts.permit || opts.votes}>
      <input type="checkbox" bind:checked={opts.permit}>
      Permit Gasless TX
      <HelpTooltip link="https://docs.openzeppelin.com/contracts/4.x/api/token/erc20#ERC20Permit">
        Without paying gas, token holders will be able to allow third parties to transfer from their account.
      </HelpTooltip>
    </label>

    <label class:checked={opts.votes}>
      <input type="checkbox" bind:checked={opts.votes}>
      Voting Ability
      <HelpTooltip link="https://docs.openzeppelin.com/contracts/4.x/api/token/erc20#ERC20Votes">
        Keeps track of historical balances for voting in on-chain governance, with a way to delegate one's voting power to a trusted account.
      </HelpTooltip>
    </label>

    <label class:checked={opts.flashmint}>
      <input type="checkbox" bind:checked={opts.flashmint}>
      Flash Loan Minting
      <HelpTooltip link="https://docs.openzeppelin.com/contracts/4.x/api/token/erc20#ERC20FlashMint">
        Built-in flash loans. Lend tokens without requiring collateral as long as they're returned in the same transaction.
      </HelpTooltip>
    </label>

    <label class:checked={opts.snapshots}>
      <input type="checkbox" bind:checked={opts.snapshots}>
      Snapshot of Holders
      <HelpTooltip link="https://docs.openzeppelin.com/contracts/4.x/api/token/erc20#ERC20Snapshot">
        Privileged accounts will be able to store snapshots of balances that can be retrieved later.
        <br>
        For on-chain voting, the Votes option is preferable.
      </HelpTooltip>
    </label>
  </div>
</section>

<AccessControlSection bind:access={opts.access} required={requireAccessControl} />

<UpgradeabilitySection bind:upgradeable={opts.upgradeable} />

<InfoSection bind:info={opts.info} />
