<template>
  <div class="about">
    <Box id="dreamlink" :title="`🤔 What is ${$config.name}?`">
      <div class="prose">
        <p>
          {{ $config.name }} is a website with a series of tools that  run entirely on the decentralized P2P network: <a href="https://docs.ipfs.io/concepts/what-is-ipfs/" target="_blank">IPFS</a>.
        </p>

        <p>
          All tools (except for search) run without central servers, on a network of thousands of nodes around the world, making it <a href="https://docs.ipfs.io/concepts/what-is-ipfs/#decentralization" target="_blank">fail-safe and censorship-proof</a>.
        </p>

        <p>
          The moment you enter {{ $config.name }} you are already connected to the IPFS network as we provide you with a node that runs in your web browser.
        </p>
      </div>
    </Box>

    <Box id="tools" title="✨ Tools available">
      <div class="prose">
        <p>All the magic happens between you and your web browser, there are no servers in between that can censor your activity.</p>

        <ul>
          <li>
            <strong>Share.</strong> Upload files or even folders, <span class="text-primary">without registration, size or bandwith limits</span>. You are your own file distributor.
          </li>
          <li>
            <strong>Search.</strong> Search for files or folders thanks to <a href="https://ipfs-search.com" target="_blank">ipfs-search.com</a>
          </li>
          <li>
            <strong>Pastebin.</strong> Create and share any text format.
          </li>
          <li>
            <strong>Chat.</strong> Create an encrypted P2P chat room for free communication between people you trust.
          </li>
        </ul>
      </div>
    </Box>

    <Box id="down" :title="`😨 What if ${$config.name} goes down?`">
      <div class="prose">
        <p>{{ $config.name }} itself is a webpage stored in IPFS!</p>

        <p>
          You can use a HTTP gateway or in case you have an IPFS node or a web browser with IPFS support you can download {{ $config.name }} on your device:
        </p>

        <Gateways />
      </div>
    </Box>

    <Box id="faq" title="❓ FAQ">
      <div class="space-y-3 prose">
        <details>
          <summary class="title">
            Is it safe to upload files in {{ $config.name }}?
          </summary>

          <p>Depends on what you consider safe.</p>

          <p>OpenDreamNet does not store your files on private servers, they stay within your web browser's storage and on IPFS nodes around the world.</p>

          <p>IPFS is a P2P protocol so please note the following:</p>

          <ul>
            <li>All data you upload to IPFS is public.</li>
            <li>If you need to upload a file with sensitive information first encrypt it with external tools.</li>
            <li>It is possible to trace a file back to the IP addresses of the nodes that distribute it, even if you use applications such as VPN or Tor.</li>
            <li>You can read more about <a href="https://docs.ipfs.io/concepts/privacy/" target="_blank">privacy and IPFS here</a>.</li>
          </ul>
        </details>

        <details>
          <summary class="title">
            Where can I see the files I have uploaded?
          </summary>

          <p>
            In your <NuxtLink to="/app/profile">
              profile
            </NuxtLink> you can find the list of pinned files.
          </p>
        </details>
      </div>
    </Box>

    <Box title="🥰 Sponsor">
      <p class="mb-3">
        Do you like {{ $config.name }}? Your sponsorship would help us to keep the project open-source and up to date!
      </p>

      <Button el="a" href="https://www.opendreamnet.com/contact" target="_blank">
        How?
      </Button>
    </Box>
  </div>
</template>

<style lang="scss" scoped>
.about {
  @apply mx-auto space-y-6 max-w-prose;

  details {
    summary {
      @apply cursor-pointer;
    }
  }
}

.tip {
  @apply text-primary cursor-help border-b border-dotted;
}
</style>
