<script lang="ts">
    //props
    let {invoiceInf, items, totalPriceSum, logoState, invoicFirstLetter} = $props()

    //getting tax and estimated total value
    let tax = $derived(totalPriceSum / 10);
    let total = $derived(totalPriceSum + tax);

    //formating values to have only 2 decimal place
    const formatedSuptotallValue = $derived(
        new Intl.NumberFormat('en-US', {
          minimumFractionDigits: 2,
          maximumFractionDigits: 2
        }).format(totalPriceSum)
    );

    const formatedTaxlValue = $derived(
        new Intl.NumberFormat('en-US', {
          minimumFractionDigits: 2,
          maximumFractionDigits: 2
        }).format(tax)
    );

    const formatedTotalValue = $derived(
        new Intl.NumberFormat('en-US', {
          minimumFractionDigits: 2,
          maximumFractionDigits: 2
        }).format(total)
    );


</script>
<div class="flex justify-between">
	<div class="flex gap-3">
		{#if logoState}
            <img src={$invoiceInf.logo} alt="invoice logo" class="w-20 h-20 rounded-lg">
            {:else}
            <div class="bg-gray-400 h-20 w-22 rounded-2xl flex justify-center items-center">
                <p class="font-bold text-black text-xl uppercase">{invoicFirstLetter}</p>
            </div>
        {/if}
        <div class="pt-4">
           <h1 class="font-semibold text-lg text-black">{$invoiceInf.invoiceFrom}</h1>
		   <p class="text-xs">123 Main Street — contact@{$invoiceInf.invoiceFrom}.example</p>
        </div>
	</div>
	<div>
       <p class="text-end">Invoice</p>
       <h1 class="font-semibold text-xl text-black text-end uppercase">{$invoiceInf.invoiceTitle}</h1>
       <p class="text-end">{$invoiceInf.invoiceDate}</p>
    </div>
</div>

<div class="grid grid-cols-1 md:grid-cols-2 gap-4">
    <div class="mt-7">
        <p>Bill To</p>
        <h1 class="font-semibold text-base text-black">{$invoiceInf.invoiceTo}</h1>
    </div>

	<div class="mt-7">
		<p>Status</p>
		<h3 class="font-semibold text-base text-blue-700">Unpaid</h3>
    </div>
</div>


	<table class="w-full text-sm mt-7 mb-2">
		<thead>
			<tr>
				<th class="px-4 py-3 text-left">Description</th>
				<th class="w-24 px-4 py-3 text-left">Qty</th>
				<th class="w-24 px-4 py-3 text-left">Price</th>
				<th class="w-36 px-4 py-3 text-right">Amount</th>
			</tr>
		</thead>
        {#each items as item (item.id) }
            <tbody class="divide-y divide-slate-100 text-black">
                <tr class="last:border-none">
                    <td class="px-3 py-2">{item.description}</td>
                    <td class="px-3 py-2">{item.qty}</td>
                    <td class="px-3 py-2">{item.price}</td>
                    <td class="px-3 py-2 text-right">${item.price * item.qty}</td>
                </tr>
            </tbody>
        {/each}
	</table>

<div class="flex justify-end gap-10">
    <div>
        <p>Subtotal</p>
        <p>Tax (10%)</p>
        <h1 class="font-semibold text-xl text-black">Total</h1>
    </div>
    <div>
        <p>${formatedSuptotallValue}</p>
        <p>${formatedTaxlValue}</p>
        <h1  class="font-semibold text-xl text-black">${formatedTotalValue}</h1>
    </div>
</div>

<footer class="text-xs mt-5">{$invoiceInf.note}</footer>

