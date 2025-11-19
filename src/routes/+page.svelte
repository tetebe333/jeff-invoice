<script lang="ts">
    import InvoiceStudio from "./Invoice-studio.svelte";


    let invoiceInf = $state({invoiceFrom:"Jeffrey", invoiceTo: "Victory", invoiceTitle:"INV-00d", invoiceDate:"2025-11-17", note:"Thank you for your business. Payment due within 14 days.", logo:"src/lib/assets/ac.jpeg"})
    let selectedFile = $state(null)
    let items:any = $state([])

    //total price for all items
    const totalPriceSum = $derived(
        items.reduce((sum:any, currentItem:any) => sum + currentItem.price * currentItem.qty, 0)
    );
    
    //logo function
    const logoFunction = (event: any) => {
         const files = event.target.files;      
        if (files.length > 0 && files.length || 0) {
            selectedFile = files[0];
        } else {
            selectedFile = null;
        }

        //creating logo
        const theSelectedFile = selectedFile;   
        if (theSelectedFile) {
            const reader = new FileReader();
            reader.onload = function (e:any) {
                const fileContent = e.target.result;
                 
                invoiceInf.logo = fileContent;  
            }

            reader.readAsDataURL(theSelectedFile);
        }
    }

    //creating items function
    let itemsLength = 0;
    const addItemFunction = () => {
       itemsLength +=1;
       const item = {id: itemsLength, description:"New item", qty:1, price:0}
       items.push(item)
    }

</script>

<main class=" mx-auto lg:max-w-1150 w-90% max-w-900 text-slate-500 text-sm grid grid-cols-1 lg:grid-cols-[1fr_420px] gap-6">
    <div class="bg-white-o rounded-xl shadow-xl/20 ">
        <div class="px-4 py-4 ">
            <div class="flex gap-3">
                <img src={invoiceInf.logo} alt="invoice logo" class="w-20 h-20 rounded-lg">
                <div class="pt-4">
                <h1 class="font-semibold text-xl text-black">Invoice Studio</h1>
                <p>Beautiful invoices — edit, preview and export</p>
                </div>
            </div>

            <form class="mt-7 text-xs">
                <div class=" grid grid-cols-1 md:grid-cols-2 gap-4">
                    <div>
                      <label for="form">From (you / company)</label>
                      <input bind:value={invoiceInf.invoiceFrom} type="text" class="input">
                    </div>
                    
                    <div>
                      <label for="Bill To">Bill To</label>
                      <input bind:value={invoiceInf.invoiceTo} type="text" class="input">
                    </div>

                    <div>
                      <label for="Invoice">Invoice #</label>
                      <input bind:value={invoiceInf.invoiceTitle} type="text" class="input">
                    </div>
                    
                    <div>
                      <label for="Invoice Date">Invoice Date</label>
                      <input bind:value={invoiceInf.invoiceDate} type="date" class="input">
                    </div>
                </div>
                

                <label for="Notes">Notes</label>
                <textarea bind:value={invoiceInf.note} name="Notes" id="" class="areaInput"></textarea>

                <div class="rounded-xl border border-slate-100 overflow-hidden mb-4">

                    <table class="w-full text-sm">
                        <thead class="bg-white">
                            <tr>
                                <th class="px-4 py-3 text-left">Description</th>
                                <th class="px-4 py-3 text-left w-24">Qty</th>
                                <th class="px-4 py-3 text-left w-24">Price</th>
                                <th class="px-4 py-3 t w-36">Amount</th>
                            </tr>
                        </thead>
                        {#each items as item, index (item.id)}
                            <tbody class=" {index} bg-white divide-y divide-slate-100">
                                <tr class="last:border-none">
                                    <td class="ps-5 py-2">
                                        <input type="text" bind:value={item.description} data-field="desc" class="w-full text-small outline-none">
                                    </td>
                                    <td class="ps-5 py-2">
                                        <input  type="number" bind:value={item.qty} data-field="qty" min="0" class="w-full text-sm outline-none">
                                    </td>
                                    <td class="ps-5 py-2">
                                        <input type="number" bind:value={item.price} data-field="price" min="0" step="0.01" class="w-full text-sm outline-none">
                                    </td>
                                    <td class="px-3 py-2 text-right">
                                        ${item.price * item.qty} 
                                        <button onclick= {() => items.splice(index, 1)} class="ml-2 text-xs text-slate-500">Delete</button>
                                    </td>
                                </tr>
                            </tbody>
                        {/each}
                    </table>
                
                    <div class="flex justify-end p-3 bg-slate-50">
                        <button onclick={addItemFunction} type="button" class="rounded-lg px-3 py-2 border border-slate-200 text-sm">+ Add item</button>
                    </div>
                </div>

                <div class="sm:flex items-center justify-between gap-3 text-black w-100%">
                    <div class="flex items-center gap-2">
                        <label for="logo" class="text-xs text-slate-500">Logo</label>
                        <input onchange={logoFunction} type="file" accept="image/*" class="text-xs">
                        <button onclick={() => invoiceInf.logo = "src/lib/assets/ac.jpeg"} type="button" class="rounded-xl bg-white px-4 py-2 border border-slate-200 text-sm " >Reset</button>
                    </div>
                    <div class="flex items-center gap-2 mt-5 sm:mt-0">
                        <button onclick={() => alert("Saved locally in your browser.")} type="button" class="rounded-xl bg-white px-4 py-2 border border-slate-200 text-sm ">Save</button>
                        <button onclick={()=> window.print()} type="button" class="rounded-xl bg-linear-to-r from-indigo-600 to-teal-400 text-white px-4 py-2 text-sm">Print / Explort</button>
                    </div>
                </div>
            </form>

      </div>  
    </div>
   
    <div id="printable-section" class="bg-white-o rounded-xl mt-6 lg:mt-0 text-sm shadow-xl/20">
        <div class="px-4 py-4">
          <InvoiceStudio {invoiceInf} {items} {totalPriceSum}/>
        </div>
    </div> 
      
</main>

<style>
  @media print {
        :global(body) {
          visibility: hidden; 
        }

        #printable-section {
            visibility: visible;
            position: absolute;
            width: 90%;
            max-width: 900px;   
        }
  }
</style>