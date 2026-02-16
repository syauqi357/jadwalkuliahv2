<script>
  import jadwalData from '../data/jadwal-kuliah.json';

  const data = jadwalData;

  const dayConfig = {
    Senin:  { dot: 'bg-blue-500',    text: 'text-blue-600',    sksBg: 'bg-blue-50 text-blue-600 ring-blue-200' },
    Selasa: { dot: 'bg-emerald-500', text: 'text-emerald-600', sksBg: 'bg-emerald-50 text-emerald-600 ring-emerald-200' },
    Rabu:   { dot: 'bg-amber-500',   text: 'text-amber-600',   sksBg: 'bg-amber-50 text-amber-600 ring-amber-200' },
    Kamis:  { dot: 'bg-violet-500',  text: 'text-violet-600',  sksBg: 'bg-violet-50 text-violet-600 ring-violet-200' },
    Jumat:  { dot: 'bg-rose-500',    text: 'text-rose-600',    sksBg: 'bg-rose-50 text-rose-600 ring-rose-200' },
  };

  const fallback = { dot: 'bg-gray-400', text: 'text-gray-600', sksBg: 'bg-gray-50 text-gray-600 ring-gray-200' };

  const totalSKS = $derived(data.jadwal.reduce((acc, i) => acc + (i.sks || 0), 0));
  const totalMK = $derived(data.jadwal.filter(i => i.mataKuliah).length);

  function cfg(hari) {
    return dayConfig[hari] || fallback;
  }
</script>

<!-- Outer wrapper -->
<div class="min-h-screen bg-slate-50 font-sans text-slate-800">

  <!-- Top bar -->
  <header class="sticky top-0 z-10 border-b border-slate-200/80 bg-white/80 backdrop-blur-lg">
    <div class="mx-auto flex max-w-5xl items-center justify-between px-6 py-4">
      <div>
        <h1 class="text-lg font-semibold tracking-tight text-slate-900">Jadwal Kuliah</h1>
        <p class="text-xs text-slate-400">{data.semester}</p>
      </div>
      <span class="rounded-full border border-slate-200 bg-slate-50 px-3 py-1 text-xs font-medium text-slate-500">
        {data.program} &middot; Kelas {data.kelas}
      </span>
    </div>
  </header>

  <main class="mx-auto max-w-5xl px-4 py-8 sm:px-6">

    <!-- Stats row -->
    <div class="mb-6 grid grid-cols-2 gap-3 sm:grid-cols-4 sm:gap-4">
      <div class="rounded-xl border border-slate-200 bg-white px-4 py-3">
        <p class="text-[11px] font-medium uppercase tracking-wider text-slate-400">Total SKS</p>
        <p class="mt-1 text-2xl font-bold text-slate-900">{totalSKS}</p>
      </div>
      <div class="rounded-xl border border-slate-200 bg-white px-4 py-3">
        <p class="text-[11px] font-medium uppercase tracking-wider text-slate-400">Mata Kuliah</p>
        <p class="mt-1 text-2xl font-bold text-slate-900">{totalMK}</p>
      </div>
      <div class="rounded-xl border border-slate-200 bg-white px-4 py-3">
        <p class="text-[11px] font-medium uppercase tracking-wider text-slate-400">Hari Aktif</p>
        <p class="mt-1 text-2xl font-bold text-slate-900">{data.jadwal.filter(i => i.mataKuliah).length}</p>
      </div>
      <div class="rounded-xl border border-slate-200 bg-white px-4 py-3">
        <p class="text-[11px] font-medium uppercase tracking-wider text-slate-400">Semester</p>
        <p class="mt-1 text-2xl font-bold text-slate-900">8</p>
      </div>
    </div>

    <!-- Mobile hint -->
    <p class="mb-3 flex items-center gap-1.5 text-xs text-slate-400 sm:hidden">
      <svg class="h-3.5 w-3.5" fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="2"><path stroke-linecap="round" stroke-linejoin="round" d="M9 5l7 7-7 7"/></svg>
      Scroll ke kanan
    </p>

    <!-- Table card -->
    <div class="overflow-hidden rounded-2xl border border-slate-200 bg-white shadow-sm">
      <div class="overflow-x-auto">
        <table class="w-full min-w-[680px]">
          <thead>
            <tr class="border-b border-slate-100 bg-slate-50/60 text-left text-[11px] font-semibold uppercase tracking-wider text-slate-400">
              <th class="py-3 pl-5 pr-3">Hari</th>
              <th class="px-3 py-3">Jam</th>
              <th class="px-3 py-3">Kode MK</th>
              <th class="px-3 py-3">Mata Kuliah</th>
              <th class="px-3 py-3">SKS</th>
              <th class="px-3 py-3">Dosen Pengampu</th>
              <th class="py-3 pl-3 pr-5">Ruang</th>
            </tr>
          </thead>
          <tbody>
            {#each data.jadwal as item, i}
              {@const c = cfg(item.hari)}
              {@const empty = !item.mataKuliah}
              <tr class="group border-b border-slate-50 transition-colors last:border-0 hover:bg-slate-50/50 {empty ? 'opacity-40' : ''}">
                <!-- Hari -->
                <td class="py-3.5 pl-5 pr-3">
                  <div class="flex items-center gap-2.5">
                    <span class="inline-block h-2 w-2 rounded-full {c.dot}"></span>
                    <span class="text-sm font-semibold {c.text}">{item.hari}</span>
                  </div>
                </td>
                <!-- Jam -->
                <td class="px-3 py-3.5">
                  {#if item.jam}
                    <span class="text-sm tabular-nums text-slate-600">{item.jam}</span>
                  {:else}
                    <span class="text-sm text-slate-300">&mdash;</span>
                  {/if}
                </td>
                <!-- Kode MK -->
                <td class="px-3 py-3.5">
                  {#if item.kodeMK}
                    <code class="rounded-md bg-slate-100 px-2 py-0.5 text-[12px] font-mono font-medium text-slate-500">{item.kodeMK}</code>
                  {:else}
                    <span class="text-sm text-slate-300">&mdash;</span>
                  {/if}
                </td>
                <!-- Mata Kuliah -->
                <td class="px-3 py-3.5">
                  <span class="text-sm font-medium text-slate-800">{item.mataKuliah || '\u2014'}</span>
                </td>
                <!-- SKS -->
                <td class="px-3 py-3.5">
                  {#if item.sks > 0}
                    <span class="inline-flex h-6 w-6 items-center justify-center rounded-md ring-1 text-[12px] font-bold {c.sksBg}">
                      {item.sks}
                    </span>
                  {:else}
                    <span class="text-sm text-slate-300">&mdash;</span>
                  {/if}
                </td>
                <!-- Dosen -->
                <td class="px-3 py-3.5">
                  <span class="text-sm text-slate-500">{item.dosen || '\u2014'}</span>
                </td>
                <!-- Ruang -->
                <td class="py-3.5 pl-3 pr-5">
                  {#if item.ruang}
                    <span class="inline-flex items-center rounded-md border border-slate-200 bg-slate-50 px-2 py-0.5 text-[12px] font-medium text-slate-600">
                      {item.ruang}
                    </span>
                  {:else}
                    <span class="text-sm text-slate-300">&mdash;</span>
                  {/if}
                </td>
              </tr>
            {/each}
          </tbody>
        </table>
      </div>
    </div>

    <!-- Footer -->
    <p class="mt-6 text-center text-[11px] text-slate-300">
      {data.semester} &middot; Jadwal dapat berubah sewaktu-waktu
    </p>
  </main>
</div>