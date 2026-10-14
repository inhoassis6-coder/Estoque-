# Estoque-
<!DOCTYPE html>
<html lang="pt-BR" class="dark">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>BarberStock - Gestão de Estoque de Barbearia</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700&display=swap" rel="stylesheet">
    
    <script>
        tailwind.config = {
            darkMode: 'class',
            theme: {
                extend: {
                    colors: {
                        amber: {
                            400: '#fbbf24',
                            500: '#f59e0b',
                            600: '#d97706',
                        },
                        dark: {
                            900: '#121214',
                            800: '#1a1a1e',
                            700: '#26262c',
                            600: '#32323a'
                        }
                    },
                    fontFamily: {
                        sans: ['Inter', 'sans-serif'],
                    }
                }
            }
        }
    </script>
    
    <style>
        body {
            font-family: 'Inter', sans-serif;
            background-color: #121214;
            color: #e4e4e7;
        }
        /* Custom Scrollbar */
        ::-webkit-scrollbar {
            width: 8px;
            height: 8px;
        }
        ::-webkit-scrollbar-track {
            background: #1a1a1e;
        }
        ::-webkit-scrollbar-thumb {
            background: #32323a;
            border-radius: 4px;
        }
        ::-webkit-scrollbar-thumb:hover {
            background: #d97706;
        }
    </style>
</head>
<body class="min-h-screen flex flex-col bg-dark-900 text-gray-100">

    <!-- Header/Navbar -->
    <header class="bg-dark-800 border-b border-dark-700 sticky top-0 z-30 shadow-lg">
        <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-4 flex justify-between items-center">
            <div class="flex items-center space-x-3">
                <div class="bg-amber-500 text-dark-900 p-2.5 rounded-xl font-bold text-xl shadow-md shadow-amber-500/20">
                    <i class="fa-solid fa-scissors"></i>
                </div>
                <div>
                    <h1 class="text-xl font-bold tracking-tight text-white flex items-center gap-2">
                        BarberStock
                        <span class="text-xs px-2 py-0.5 rounded bg-amber-500/10 text-amber-400 border border-amber-500/20 font-medium">PRO</span>
                    </h1>
                    <p class="text-xs text-gray-400">Controle de Estoque & Vendas</p>
                </div>
            </div>
            
            <div class="flex items-center space-x-3">
                <button onclick="openModal('saleModal')" class="bg-gradient-to-r from-amber-500 to-amber-600 hover:from-amber-600 hover:to-amber-700 text-dark-900 font-semibold px-4 py-2 rounded-lg text-sm transition-all duration-200 flex items-center gap-2 shadow-lg shadow-amber-500/10 active:scale-95">
                    <i class="fa-solid fa-cart-shopping"></i>
                    <span class="hidden sm:inline">Nova Venda</span>
                </button>
                <button onclick="openModal('productModal')" class="bg-dark-700 hover:bg-dark-600 border border-dark-600 text-white font-medium px-4 py-2 rounded-lg text-sm transition-all duration-200 flex items-center gap-2 active:scale-95">
                    <i class="fa-solid fa-plus text-amber-400"></i>
                    <span class="hidden sm:inline">Cadastrar Produto</span>
                </button>
                <button onclick="openModal('historyModal')" class="bg-dark-700 hover:bg-dark-600 border border-dark-600 text-gray-300 font-medium px-3 py-2 rounded-lg text-sm transition-all duration-200 flex items-center gap-2 active:scale-95" title="Histórico de Vendas">
                    <i class="fa-solid fa-clock-rotate-left text-amber-400"></i>
                    <span class="hidden md:inline">Histórico</span>
                </button>
                <button onclick="openModal('dataModal')" class="bg-dark-700 hover:bg-dark-600 border border-dark-600 text-gray-300 font-medium px-3 py-2 rounded-lg text-sm transition-all duration-200 flex items-center gap-2 active:scale-95" title="Backup / Opções">
                    <i class="fa-solid fa-gear text-amber-400"></i>
                </button>
            </div>
        </div>
    </header>

    <!-- Main Content -->
    <main class="flex-grow max-w-7xl w-full mx-auto px-4 sm:px-6 lg:px-8 py-8 space-y-8">

        <!-- Dashboard / Cards de Resumo Expandido -->
        <section class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-4">
            <!-- Total de Produtos -->
            <div class="bg-dark-800 border border-dark-700 rounded-xl p-5 shadow-sm hover:border-dark-600 transition-all">
                <div class="flex justify-between items-start">
                    <div>
                        <p class="text-xs font-medium text-gray-400 uppercase tracking-wider">Produtos Cadastrados</p>
                        <h3 id="dashTotalProducts" class="text-2xl font-bold text-white mt-2">0</h3>
                        <p id="dashCategoriesCount" class="text-xs text-gray-400 mt-1">0 categorias</p>
                    </div>
                    <div class="p-3 bg-blue-500/10 text-blue-400 rounded-lg">
                        <i class="fa-solid fa-boxes-stacked text-xl"></i>
                    </div>
                </div>
            </div>

            <!-- Itens em Estoque e Valor de Custo -->
            <div class="bg-dark-800 border border-dark-700 rounded-xl p-5 shadow-sm hover:border-dark-600 transition-all">
                <div class="flex justify-between items-start">
                    <div>
                        <p class="text-xs font-medium text-gray-400 uppercase tracking-wider">Patrimônio em Estoque</p>
                        <h3 id="dashTotalStockCost" class="text-2xl font-bold text-white mt-2">R$ 0,00</h3>
                        <p id="dashTotalStock" class="text-xs text-gray-400 mt-1">0 unidades totais</p>
                    </div>
                    <div class="p-3 bg-emerald-500/10 text-emerald-400 rounded-lg">
                        <i class="fa-solid fa-vault text-xl"></i>
                    </div>
                </div>
            </div>

            <!-- Estoque Baixo / Alerta -->
            <div class="bg-dark-800 border border-dark-700 rounded-xl p-5 shadow-sm hover:border-dark-600 transition-all">
                <div class="flex justify-between items-start">
                    <div>
                        <p class="text-xs font-medium text-gray-400 uppercase tracking-wider">Reposição Necessária</p>
                        <h3 id="dashLowStock" class="text-2xl font-bold text-amber-400 mt-2">0 <span class="text-xs text-gray-400 font-normal">itens</span></h3>
                        <p id="dashOutStock" class="text-xs text-red-400 mt-1">0 esgotados</p>
                    </div>
                    <div class="p-3 bg-amber-500/10 text-amber-400 rounded-lg">
                        <i class="fa-solid fa-triangle-exclamation text-xl"></i>
                    </div>
                </div>
            </div>

            <!-- Total de Vendas / Lucro estimado -->
            <div class="bg-dark-800 border border-dark-700 rounded-xl p-5 shadow-sm hover:border-dark-600 transition-all">
                <div class="flex justify-between items-start">
                    <div>
                        <p class="text-xs font-medium text-gray-400 uppercase tracking-wider">Faturamento / Lucro</p>
                        <h3 id="dashTotalSales" class="text-2xl font-bold text-emerald-400 mt-2">R$ 0,00</h3>
                        <p id="dashProfit" class="text-xs text-emerald-500 font-medium mt-1">Lucro Estimado: R$ 0,00</p>
                    </div>
                    <div class="p-3 bg-emerald-500/10 text-emerald-400 rounded-lg">
                        <i class="fa-solid fa-chart-line text-xl"></i>
                    </div>
                </div>
            </div>
        </section>

        <!-- Filtros e Busca -->
        <section class="bg-dark-800 border border-dark-700 rounded-xl p-4 flex flex-col md:flex-row gap-4 justify-between items-center shadow-sm">
            <div class="relative w-full md:w-96">
                <i class="fa-solid fa-magnifying-glass absolute left-3 top-1/2 -translate-y-1/2 text-gray-400"></i>
                <input type="text" id="searchInput" oninput="renderTable()" placeholder="Buscar produto..." class="w-full bg-dark-900 border border-dark-600 rounded-lg pl-10 pr-4 py-2 text-sm text-white focus:outline-none focus:border-amber-500 transition-colors">
            </div>

            <div class="flex flex-wrap items-center gap-3 w-full md:w-auto">
                <select id="categoryFilter" onchange="renderTable()" class="bg-dark-900 border border-dark-600 rounded-lg px-3 py-2 text-sm text-white focus:outline-none focus:border-amber-500 transition-colors flex-grow md:flex-grow-0">
                    <option value="ALL">Todas as Categorias</option>
                    <option value="Pomadas">Pomadas / Ceras</option>
                    <option value="Shampoos">Shampoos / Condicionadores</option>
                    <option value="Óleos">Óleos & Barba</option>
                    <option value="Lâminas">Lâminas & Descartáveis</option>
                    <option value="Bebidas">Cervejas & Bebidas</option>
                    <option value="Outros">Outros</option>
                </select>

                <select id="statusFilter" onchange="renderTable()" class="bg-dark-900 border border-dark-600 rounded-lg px-3 py-2 text-sm text-white focus:outline-none focus:border-amber-500 transition-colors flex-grow md:flex-grow-0">
                    <option value="ALL">Todos os Status</option>
                    <option value="LOW">Apenas Estoque Baixo</option>
                    <option value="OUT">Esgotados</option>
                    <option value="OK">Estoque Normal</option>
                </select>
            </div>
        </section>

        <!-- Tabela de Gestão de Estoque -->
        <section class="bg-dark-800 border border-dark-700 rounded-xl overflow-hidden shadow-lg">
            <div class="px-6 py-4 border-b border-dark-700 flex justify-between items-center">
                <h2 class="text-lg font-semibold text-white flex items-center gap-2">
                    <i class="fa-solid fa-list-check text-amber-500"></i>
                    Lista Detalhada de Estoque
                </h2>
                <span id="productCounterBadge" class="text-xs bg-dark-700 text-gray-300 font-medium px-2.5 py-1 rounded-full border border-dark-600">0 exibidos</span>
            </div>

            <div class="overflow-x-auto">
                <table class="w-full text-left border-collapse">
                    <thead>
                        <tr class="bg-dark-900/50 text-gray-400 text-xs font-semibold uppercase tracking-wider border-b border-dark-700">
                            <th class="py-3 px-6">Produto</th>
                            <th class="py-3 px-6">Categoria</th>
                            <th class="py-3 px-6 text-center">Em Estoque</th>
                            <th class="py-3 px-6 text-center">Status / Meta</th>
                            <th class="py-3 px-6 text-center">Vendidos</th>
                            <th class="py-3 px-6 text-right">Custo / Venda</th>
                            <th class="py-3 px-6 text-right">Lucro Un.</th>
                            <th class="py-3 px-6 text-center">Ações</th>
                        </tr>
                    </thead>
                    <tbody id="inventoryTableBody" class="divide-y divide-dark-700/60 text-sm">
                        <!-- Conteúdo carregado via JS -->
                    </tbody>
                </table>
            </div>

            <div id="emptyState" class="hidden text-center py-12 px-4">
                <i class="fa-solid fa-box-open text-4xl text-gray-600 mb-3"></i>
                <p class="text-gray-400 font-medium">Nenhum produto encontrado com os filtros atuais.</p>
                <p class="text-xs text-gray-500 mt-1">Tente ajustar a busca ou cadastrar novos itens.</p>
            </div>
        </section>

    </main>

    <!-- Modal: Detalhes do Produto / Ajustes Avançados -->
    <div id="detailModal" class="fixed inset-0 bg-black/70 backdrop-blur-sm flex items-center justify-center p-4 z-50 hidden transition-opacity">
        <div class="bg-dark-800 border border-dark-700 rounded-xl w-full max-w-lg overflow-hidden shadow-2xl">
            <div class="px-6 py-4 border-b border-dark-700 flex justify-between items-center bg-dark-900/40">
                <h3 class="text-lg font-bold text-white flex items-center gap-2">
                    <i class="fa-solid fa-circle-info text-amber-500"></i>
                    Detalhes e Ajustes
                </h3>
                <button onclick="closeModal('detailModal')" class="text-gray-400 hover:text-white transition-colors">
                    <i class="fa-solid fa-xmark text-xl"></i>
                </button>
            </div>
            
            <div id="detailModalBody" class="p-6 space-y-5">
                <!-- Conteúdo dinâmico via JS -->
            </div>
            
            <div class="px-6 py-4 bg-dark-900/40 border-t border-dark-700 flex justify-between items-center">
                <button id="btnDeleteFromDetail" class="text-red-400 hover:text-red-300 text-sm font-medium flex items-center gap-1.5">
                    <i class="fa-solid fa-trash"></i> Excluir Produto
                </button>
                <button onclick="closeModal('detailModal')" class="bg-dark-700 hover:bg-dark-600 text-white font-medium px-4 py-2 rounded-lg text-sm transition-colors">
                    Fechar
                </button>
            </div>
        </div>
    </div>

    <!-- Modal: Cadastrar / Editar Produto -->
    <div id="productModal" class="fixed inset-0 bg-black/70 backdrop-blur-sm flex items-center justify-center p-4 z-50 hidden transition-opacity">
        <div class="bg-dark-800 border border-dark-700 rounded-xl w-full max-w-md overflow-hidden shadow-2xl transform transition-all">
            <div class="px-6 py-4 border-b border-dark-700 flex justify-between items-center">
                <h3 id="modalTitle" class="text-lg font-bold text-white">Cadastrar Produto</h3>
                <button onclick="closeModal('productModal')" class="text-gray-400 hover:text-white transition-colors">
                    <i class="fa-solid fa-xmark text-xl"></i>
                </button>
            </div>
            
            <form id="productForm" onsubmit="handleSaveProduct(event)" class="p-6 space-y-4">
                <input type="hidden" id="productId">

                <div>
                    <label class="block text-xs font-medium text-gray-300 mb-1">Nome do Produto *</label>
                    <input type="text" id="prodName" required placeholder="Ex: Pomada Matte Effect 100g" class="w-full bg-dark-900 border border-dark-600 rounded-lg px-3 py-2 text-sm text-white focus:outline-none focus:border-amber-500">
                </div>

                <div>
                    <label class="block text-xs font-medium text-gray-300 mb-1">Categoria *</label>
                    <select id="prodCategory" required class="w-full bg-dark-900 border border-dark-600 rounded-lg px-3 py-2 text-sm text-white focus:outline-none focus:border-amber-500">
                        <option value="Pomadas">Pomadas / Ceras</option>
                        <option value="Shampoos">Shampoos / Condicionadores</option>
                        <option value="Óleos">Óleos & Barba</option>
                        <option value="Lâminas">Lâminas & Descartáveis</option>
                        <option value="Bebidas">Cervejas & Bebidas</option>
                        <option value="Outros">Outros</option>
                    </select>
                </div>

                <div class="grid grid-cols-2 gap-4">
                    <div>
                        <label class="block text-xs font-medium text-gray-300 mb-1">Estoque Atual *</label>
                        <input type="number" id="prodQuantity" min="0" required placeholder="0" class="w-full bg-dark-900 border border-dark-600 rounded-lg px-3 py-2 text-sm text-white focus:outline-none focus:border-amber-500">
                    </div>
                    <div>
                        <label class="block text-xs font-medium text-gray-300 mb-1">Estoque Mínimo (Alerta)</label>
                        <input type="number" id="prodMinQuantity" min="1" value="5" placeholder="Ex: 5" class="w-full bg-dark-900 border border-dark-600 rounded-lg px-3 py-2 text-sm text-white focus:outline-none focus:border-amber-500">
                    </div>
                </div>

                <div class="grid grid-cols-2 gap-4">
                    <div>
                        <label class="block text-xs font-medium text-gray-300 mb-1">Preço de Custo (R$)</label>
                        <input type="number" step="0.01" id="prodCostPrice" min="0" placeholder="0,00" class="w-full bg-dark-900 border border-dark-600 rounded-lg px-3 py-2 text-sm text-white focus:outline-none focus:border-amber-500">
                    </div>
                    <div>
                        <label class="block text-xs font-medium text-gray-300 mb-1">Preço de Venda (R$) *</label>
                        <input type="number" step="0.01" id="prodSellPrice" min="0" required placeholder="0,00" class="w-full bg-dark-900 border border-dark-600 rounded-lg px-3 py-2 text-sm text-white focus:outline-none focus:border-amber-500">
                    </div>
                </div>

                <div>
                    <label class="block text-xs font-medium text-gray-300 mb-1">Total de Unidades Vendidas (Histórico)</label>
                    <input type="number" id="prodSold" min="0" value="0" placeholder="0" class="w-full bg-dark-900 border border-dark-600 rounded-lg px-3 py-2 text-sm text-white focus:outline-none focus:border-amber-500">
                </div>

                <div class="pt-4 flex justify-end gap-3 border-t border-dark-700">
                    <button type="button" onclick="closeModal('productModal')" class="px-4 py-2 rounded-lg text-sm text-gray-400 hover:text-white hover:bg-dark-700 transition-colors">Cancelar</button>
                    <button type="submit" class="bg-amber-500 hover:bg-amber-600 text-dark-900 font-semibold px-5 py-2 rounded-lg text-sm transition-colors">Salvar Produto</button>
                </div>
            </form>
        </div>
    </div>

    <!-- Modal: Registrar Venda Rápida -->
    <div id="saleModal" class="fixed inset-0 bg-black/70 backdrop-blur-sm flex items-center justify-center p-4 z-50 hidden transition-opacity">
        <div class="bg-dark-800 border border-dark-700 rounded-xl w-full max-w-md overflow-hidden shadow-2xl transform transition-all">
            <div class="px-6 py-4 border-b border-dark-700 flex justify-between items-center">
                <h3 class="text-lg font-bold text-white flex items-center gap-2">
                    <i class="fa-solid fa-cart-arrow-down text-amber-500"></i>
                    Registrar Nova Venda
                </h3>
                <button onclick="closeModal('saleModal')" class="text-gray-400 hover:text-white transition-colors">
                    <i class="fa-solid fa-xmark text-xl"></i>
                </button>
            </div>
            
            <form id="saleForm" onsubmit="handleQuickSale(event)" class="p-6 space-y-4">
                <div>
                    <label class="block text-xs font-medium text-gray-300 mb-1">Selecione o Produto *</label>
                    <select id="saleSelectProduct" onchange="updateSalePriceInfo()" required class="w-full bg-dark-900 border border-dark-600 rounded-lg px-3 py-2 text-sm text-white focus:outline-none focus:border-amber-500">
                        <!-- Populado via JS -->
                    </select>
                </div>

                <div class="grid grid-cols-2 gap-4">
                    <div>
                        <label class="block text-xs font-medium text-gray-300 mb-1">Quantidade Vendida *</label>
                        <input type="number" id="saleQuantity" min="1" value="1" onchange="updateSalePriceInfo()" oninput="updateSalePriceInfo()" required class="w-full bg-dark-900 border border-dark-600 rounded-lg px-3 py-2 text-sm text-white focus:outline-none focus:border-amber-500">
                    </div>
                    <div>
                        <label class="block text-xs font-medium text-gray-300 mb-1">Total a Cobrar</label>
                        <div id="saleTotalDisplay" class="w-full bg-dark-900/50 border border-dark-700 rounded-lg px-3 py-2 text-sm font-bold text-emerald-400 flex items-center">
                            R$ 0,00
                        </div>
                    </div>
                </div>

                <div class="pt-4 flex justify-end gap-3 border-t border-dark-700">
                    <button type="button" onclick="closeModal('saleModal')" class="px-4 py-2 rounded-lg text-sm text-gray-400 hover:text-white hover:bg-dark-700 transition-colors">Cancelar</button>
                    <button type="submit" class="bg-gradient-to-r from-emerald-500 to-emerald-600 hover:from-emerald-600 hover:to-emerald-700 text-white font-semibold px-5 py-2 rounded-lg text-sm transition-colors shadow-lg shadow-emerald-500/20">Finalizar Venda</button>
                </div>
            </form>
        </div>
    </div>

    <!-- Modal: Histórico de Vendas -->
    <div id="historyModal" class="fixed inset-0 bg-black/70 backdrop-blur-sm flex items-center justify-center p-4 z-50 hidden transition-opacity">
        <div class="bg-dark-800 border border-dark-700 rounded-xl w-full max-w-2xl overflow-hidden shadow-2xl flex flex-col max-h-[85vh]">
            <div class="px-6 py-4 border-b border-dark-700 flex justify-between items-center bg-dark-900/40">
                <h3 class="text-lg font-bold text-white flex items-center gap-2">
                    <i class="fa-solid fa-clock-rotate-left text-amber-500"></i>
                    Histórico de Transações de Vendas
                </h3>
                <button onclick="closeModal('historyModal')" class="text-gray-400 hover:text-white transition-colors">
                    <i class="fa-solid fa-xmark text-xl"></i>
                </button>
            </div>
            
            <div class="p-6 overflow-y-auto flex-grow space-y-3">
                <div id="salesHistoryList" class="space-y-2">
                    <!-- Registros inseridos via JS -->
                </div>
            </div>

            <div class="px-6 py-4 bg-dark-900/40 border-t border-dark-700 flex justify-between items-center">
                <button onclick="clearSalesHistory()" class="text-red-400 hover:text-red-300 text-xs font-medium flex items-center gap-1.5">
                    <i class="fa-solid fa-trash"></i> Limpar Histórico de Vendas
                </button>
                <button onclick="closeModal('historyModal')" class="bg-dark-700 hover:bg-dark-600 text-white font-medium px-4 py-2 rounded-lg text-sm transition-colors">
                    Fechar
                </button>
            </div>
        </div>
    </div>

    <!-- Modal: Backup / Restaurar Dados -->
    <div id="dataModal" class="fixed inset-0 bg-black/70 backdrop-blur-sm flex items-center justify-center p-4 z-50 hidden transition-opacity">
        <div class="bg-dark-800 border border-dark-700 rounded-xl w-full max-w-md overflow-hidden shadow-2xl">
            <div class="px-6 py-4 border-b border-dark-700 flex justify-between items-center">
                <h3 class="text-lg font-bold text-white flex items-center gap-2">
                    <i class="fa-solid fa-sliders text-amber-500"></i>
                    Gerenciar Dados do Sistema
                </h3>
                <button onclick="closeModal('dataModal')" class="text-gray-400 hover:text-white transition-colors">
                    <i class="fa-solid fa-xmark text-xl"></i>
                </button>
            </div>
            
            <div class="p-6 space-y-4">
                <p class="text-xs text-gray-400">Exporte ou importe seus produtos e histórico de vendas para não perder informações ao trocar de computador ou navegador.</p>
                
                <div class="space-y-2">
                    <button onclick="exportData()" class="w-full bg-dark-700 hover:bg-dark-600 text-white border border-dark-600 py-2.5 rounded-lg text-sm font-medium flex items-center justify-center gap-2 transition-colors">
                        <i class="fa-solid fa-download text-amber-400"></i> Exportar Backup (JSON)
                    </button>
                    
                    <label class="w-full bg-dark-700 hover:bg-dark-600 text-white border border-dark-600 py-2.5 rounded-lg text-sm font-medium flex items-center justify-center gap-2 transition-colors cursor-pointer">
                        <i class="fa-solid fa-upload text-blue-400"></i> Importar Backup
                        <input type="file" id="importFileInput" accept=".json" onchange="importData(event)" class="hidden">
                    </label>
                </div>

                <div class="border-t border-dark-700 pt-4">
                    <button onclick="resetToDefaultData()" class="w-full bg-red-500/10 hover:bg-red-500/20 text-red-400 border border-red-500/20 py-2.5 rounded-lg text-xs font-semibold flex items-center justify-center gap-2 transition-colors">
                        <i class="fa-solid fa-rotate-left"></i> Restaurar Dados de Exemplo
                    </button>
                </div>
            </div>
            
            <div class="px-6 py-3 bg-dark-900/40 border-t border-dark-700 text-right">
                <button onclick="closeModal('dataModal')" class="bg-dark-700 hover:bg-dark-600 text-white font-medium px-4 py-2 rounded-lg text-sm transition-colors">
                    Fechar
                </button>
            </div>
        </div>
    </div>

    <script>
        // Chaves de armazenamento no localStorage
        const STORAGE_KEY = 'barberstock_products_v2';
        const SALES_KEY = 'barberstock_sales_history_v2';

        // Produtos Padrão Iniciais
        const initialProducts = [
            { id: '1', name: 'Pomada Efeito Matte Premium 100g', category: 'Pomadas', stock: 12, minStock: 5, sold: 18, costPrice: 15.00, sellPrice: 35.00 },
            { id: '2', name: 'Óleo para Barba Hidratante 30ml', category: 'Óleos', stock: 3, minStock: 5, sold: 10, costPrice: 12.50, sellPrice: 30.00 },
            { id: '3', name: 'Shampoo Ice Barba & Cabelo 250ml', category: 'Shampoos', stock: 8, minStock: 4, sold: 15, costPrice: 18.00, sellPrice: 42.00 },
            { id: '4', name: 'Caixa de Lâminas Wilkinson (100un)', category: 'Lâminas', stock: 2, minStock: 3, sold: 5, costPrice: 25.00, sellPrice: 50.00 },
            { id: '5', name: 'Cerveja Long Neck Stella Artois', category: 'Bebidas', stock: 24, minStock: 12, sold: 42, costPrice: 4.50, sellPrice: 10.00 },
            { id: '6', name: 'Balm para Barba Modelador 90g', category: 'Óleos', stock: 0, minStock: 4, sold: 12, costPrice: 14.00, sellPrice: 32.00 }
        ];

        let products = [];
        let salesHistory = [];

        // Inicialização
        window.onload = function() {
            loadProducts();
            loadSalesHistory();
            renderTable();
            updateDashboard();
        };

        // Carregar do localStorage
        function loadProducts() {
            const data = localStorage.getItem(STORAGE_KEY);
            if (data) {
                try {
                    products = JSON.parse(data);
                } catch (e) {
                    console.error('Erro ao ler produtos', e);
                    products = initialProducts;
                }
            } else {
                products = initialProducts;
                saveProducts();
            }
        }

        function loadSalesHistory() {
            const data = localStorage.getItem(SALES_KEY);
            if (data) {
                try {
                    salesHistory = JSON.parse(data);
                } catch (e) {
                    salesHistory = [];
                }
            } else {
                salesHistory = [];
            }
        }

        // Salvar no localStorage
        function saveProducts() {
            localStorage.setItem(STORAGE_KEY, JSON.stringify(products));
        }

        function saveSalesHistory() {
            localStorage.setItem(SALES_KEY, JSON.stringify(salesHistory));
        }

        // Formatar Moeda
        function formatCurrency(value) {
            return new Intl.NumberFormat('pt-BR', { style: 'currency', currency: 'BRL' }).format(value || 0);
        }

        // Renderizar Tabela Principal
        function renderTable() {
            const tbody = document.getElementById('inventoryTableBody');
            const emptyState = document.getElementById('emptyState');
            const search = document.getElementById('searchInput').value.toLowerCase().trim();
            const category = document.getElementById('categoryFilter').value;
            const status = document.getElementById('statusFilter').value;

            // Filtragem
            const filtered = products.filter(prod => {
                const matchesSearch = prod.name.toLowerCase().includes(search);
                const matchesCategory = category === 'ALL' || prod.category === category;
                
                let matchesStatus = true;
                if (status === 'LOW') matchesStatus = prod.stock > 0 && prod.stock <= prod.minStock;
                if (status === 'OUT') matchesStatus = prod.stock === 0;
                if (status === 'OK') matchesStatus = prod.stock > prod.minStock;

                return matchesSearch && matchesCategory && matchesStatus;
            });

            document.getElementById('productCounterBadge').innerText = `${filtered.length} de ${products.length} exibidos`;

            if (filtered.length === 0) {
                tbody.innerHTML = '';
                emptyState.classList.remove('hidden');
                return;
            }

            emptyState.classList.add('hidden');
            
            tbody.innerHTML = filtered.map(prod => {
                let statusBadge = '';
                let faltamText = '';

                if (prod.stock === 0) {
                    statusBadge = `<span class="inline-flex items-center gap-1 px-2.5 py-1 rounded-full text-xs font-semibold bg-red-500/10 text-red-400 border border-red-500/20"><i class="fa-solid fa-circle-xmark"></i> Esgotado</span>`;
                    faltamText = `<span class="text-xs text-red-400 font-semibold block mt-0.5">Repor ${prod.minStock} un.</span>`;
                } else if (prod.stock <= prod.minStock) {
                    const diff = prod.minStock - prod.stock + 1;
                    statusBadge = `<span class="inline-flex items-center gap-1 px-2.5 py-1 rounded-full text-xs font-semibold bg-amber-500/10 text-amber-400 border border-amber-500/20"><i class="fa-solid fa-triangle-exclamation"></i> Baixo</span>`;
                    faltamText = `<span class="text-xs text-amber-400 font-medium block mt-0.5">Faltam ${diff} un.</span>`;
                } else {
                    statusBadge = `<span class="inline-flex items-center gap-1 px-2.5 py-1 rounded-full text-xs font-medium bg-emerald-500/10 text-emerald-400 border border-emerald-500/20"><i class="fa-solid fa-check"></i> Normal</span>`;
                    faltamText = `<span class="text-xs text-gray-500 block mt-0.5">OK</span>`;
                }

                const unitProfit = (parseFloat(prod.sellPrice) || 0) - (parseFloat(prod.costPrice) || 0);

                return `
                    <tr class="hover:bg-dark-700/40 transition-colors">
                        <td class="py-3 px-6 font-medium text-white cursor-pointer hover:text-amber-400 transition-colors" onclick="openProductDetails('${prod.id}')">
                            <div class="flex items-center gap-2">
                                <span>${prod.name}</span>
                                <i class="fa-solid fa-magnifying-glass-plus text-xs text-gray-500"></i>
                            </div>
                        </td>
                        <td class="py-3 px-6 text-gray-300">
                            <span class="bg-dark-700 text-gray-300 text-xs px-2.5 py-1 rounded border border-dark-600">${prod.category}</span>
                        </td>
                        <td class="py-3 px-6 text-center">
                            <span class="text-base font-bold ${prod.stock === 0 ? 'text-red-400' : (prod.stock <= prod.minStock ? 'text-amber-400' : 'text-white')}">
                                ${prod.stock}
                            </span>
                        </td>
                        <td class="py-3 px-6 text-center">
                            <div class="flex flex-col items-center">
                                ${statusBadge}
                                ${faltamText}
                            </div>
                        </td>
                        <td class="py-3 px-6 text-center text-gray-300 font-medium">
                            ${prod.sold || 0}
                        </td>
                        <td class="py-3 px-6 text-right">
                            <span class="text-xs text-gray-400 block">Custo: ${formatCurrency(prod.costPrice)}</span>
                            <span class="font-semibold text-emerald-400 block">${formatCurrency(prod.sellPrice)}</span>
                        </td>
                        <td class="py-3 px-6 text-right font-medium text-blue-400">
                            ${formatCurrency(unitProfit)}
                        </td>
                        <td class="py-3 px-6 text-center">
                            <div class="flex items-center justify-center gap-1.5">
                                <button onclick="quickAdjustStock('${prod.id}', 1)" title="Adicionar +1 ao estoque" class="p-1.5 bg-dark-700 hover:bg-dark-600 text-emerald-400 rounded transition-colors active:scale-95">
                                    <i class="fa-solid fa-plus text-xs"></i>
                                </button>
                                <button onclick="quickAdjustStock('${prod.id}', -1)" title="Vender/Dar Baixa -1" class="p-1.5 bg-dark-700 hover:bg-dark-600 text-amber-400 rounded transition-colors active:scale-95">
                                    <i class="fa-solid fa-minus text-xs"></i>
                                </button>
                                <button onclick="editProduct('${prod.id}')" title="Editar Produto" class="p-1.5 bg-dark-700 hover:bg-dark-600 text-blue-400 rounded transition-colors active:scale-95">
                                    <i class="fa-solid fa-pen text-xs"></i>
                                </button>
                                <button onclick="deleteProduct('${prod.id}')" title="Excluir Produto" class="p-1.5 bg-dark-700 hover:bg-red-500/20 text-red-400 rounded transition-colors active:scale-95">
                                    <i class="fa-solid fa-trash text-xs"></i>
                                </button>
                            </div>
                        </td>
                    </tr>
                `;
            }).join('');
        }

        // Atualizar Métricas do Dashboard
        function updateDashboard() {
            const totalProducts = products.length;
            const categories = new Set(products.map(p => p.category)).size;
            
            const totalStockUnits = products.reduce((acc, p) => acc + (parseInt(p.stock) || 0), 0);
            const totalStockCostValue = products.reduce((acc, p) => acc + ((parseInt(p.stock) || 0) * (parseFloat(p.costPrice) || 0)), 0);

            const lowStockCount = products.filter(p => p.stock > 0 && p.stock <= p.minStock).length;
            const outOfStockCount = products.filter(p => p.stock === 0).length;
            
            const totalSalesRevenue = products.reduce((acc, p) => acc + ((parseInt(p.sold) || 0) * (parseFloat(p.sellPrice) || 0)), 0);
            const totalSalesCost = products.reduce((acc, p) => acc + ((parseInt(p.sold) || 0) * (parseFloat(p.costPrice) || 0)), 0);
            const totalProfit = totalSalesRevenue - totalSalesCost;

            document.getElementById('dashTotalProducts').innerText = totalProducts;
            document.getElementById('dashCategoriesCount').innerText = `${categories} categorias ativas`;
            
            document.getElementById('dashTotalStockCost').innerText = formatCurrency(totalStockCostValue);
            document.getElementById('dashTotalStock').innerText = `${totalStockUnits} unidades em estoque`;

            document.getElementById('dashLowStock').innerText = lowStockCount;
            document.getElementById('dashOutStock').innerText = `${outOfStockCount} esgotados`;

            document.getElementById('dashTotalSales').innerText = formatCurrency(totalSalesRevenue);
            document.getElementById('dashProfit').innerText = `Lucro Líquido: ${formatCurrency(totalProfit)}`;
        }

        // Ajuste Rápido de Estoque (+1 / -1)
        function quickAdjustStock(id, amount) {
            const index = products.findIndex(p => p.id === id);
            if (index !== -1) {
                if (amount < 0) {
                    if (products[index].stock <= 0) return;
                    products[index].stock += amount;
                    products[index].sold = (products[index].sold || 0) + 1;

                    // Registra no histórico
                    recordSaleTransaction(products[index], 1);
                } else {
                    products[index].stock += amount;
                }
                saveProducts();
                renderTable();
                updateDashboard();
            }
        }

        // Abrir Modal
        function openModal(modalId) {
            if (modalId === 'saleModal') {
                populateSaleSelect();
            } else if (modalId === 'historyModal') {
                renderSalesHistory();
            }
            document.getElementById(modalId).classList.remove('hidden');
        }

        // Fechar Modal
        function closeModal(modalId) {
            document.getElementById(modalId).classList.add('hidden');
            if (modalId === 'productModal') {
                document.getElementById('productForm').reset();
                document.getElementById('productId').value = '';
                document.getElementById('modalTitle').innerText = 'Cadastrar Produto';
            }
        }

        // Salvar / Editar Produto
        function handleSaveProduct(e) {
            e.preventDefault();
            const id = document.getElementById('productId').value;
            const name = document.getElementById('prodName').value.trim();
            const category = document.getElementById('prodCategory').value;
            const stock = parseInt(document.getElementById('prodQuantity').value) || 0;
            const minStock = parseInt(document.getElementById('prodMinQuantity').value) || 5;
            const costPrice = parseFloat(document.getElementById('prodCostPrice').value) || 0;
            const sellPrice = parseFloat(document.getElementById('prodSellPrice').value) || 0;
            const sold = parseInt(document.getElementById('prodSold').value) || 0;

            if (id) {
                const index = products.findIndex(p => p.id === id);
                if (index !== -1) {
                    products[index] = {
                        ...products[index],
                        name,
                        category,
                        stock,
                        minStock,
                        costPrice,
                        sellPrice,
                        sold
                    };
                }
            } else {
                const newProduct = {
                    id: Date.now().toString(),
                    name,
                    category,
                    stock,
                    minStock,
                    sold,
                    costPrice,
                    sellPrice
                };
                products.push(newProduct);
            }

            saveProducts();
            renderTable();
            updateDashboard();
            closeModal('productModal');
        }

        // Editar Produto
        function editProduct(id) {
            const prod = products.find(p => p.id === id);
            if (!prod) return;

            document.getElementById('productId').value = prod.id;
            document.getElementById('prodName').value = prod.name;
            document.getElementById('prodCategory').value = prod.category;
            document.getElementById('prodQuantity').value = prod.stock;
            document.getElementById('prodMinQuantity').value = prod.minStock;
            document.getElementById('prodCostPrice').value = prod.costPrice;
            document.getElementById('prodSellPrice').value = prod.sellPrice;
            document.getElementById('prodSold').value = prod.sold || 0;

            document.getElementById('modalTitle').innerText = 'Editar Produto';
            openModal('productModal');
        }

        // Excluir Produto
        function deleteProduct(id) {
            if (confirm('Tem certeza que deseja remover este produto do estoque?')) {
                products = products.filter(p => p.id !== id);
                saveProducts();
                renderTable();
                updateDashboard();
            }
        }

        // Ver Detalhes do Produto
        function openProductDetails(id) {
            const prod = products.find(p => p.id === id);
            if (!prod) return;

            const unitProfit = (parseFloat(prod.sellPrice) || 0) - (parseFloat(prod.costPrice) || 0);
            const totalRevenue = (prod.sold || 0) * (parseFloat(prod.sellPrice) || 0);
            const totalProfit = (prod.sold || 0) * unitProfit;
            const marginPercent = prod.sellPrice > 0 ? ((unitProfit / prod.sellPrice) * 100).toFixed(1) : 0;

            const body = document.getElementById('detailModalBody');
            body.innerHTML = `
                <div>
                    <h4 class="text-xl font-bold text-white">${prod.name}</h4>
                    <span class="inline-block mt-1 bg-dark-700 text-gray-300 text-xs px-2.5 py-1 rounded border border-dark-600">${prod.category}</span>
                </div>

                <div class="grid grid-cols-2 gap-3 bg-dark-900/60 p-4 rounded-lg border border-dark-700">
                    <div>
                        <span class="text-xs text-gray-400 block">Estoque Atual</span>
                        <span class="text-lg font-bold ${prod.stock === 0 ? 'text-red-400' : 'text-white'}">${prod.stock} un.</span>
                    </div>
                    <div>
                        <span class="text-xs text-gray-400 block">Estoque Mínimo</span>
                        <span class="text-lg font-bold text-gray-300">${prod.minStock} un.</span>
                    </div>
                    <div>
                        <span class="text-xs text-gray-400 block">Preço de Custo</span>
                        <span class="text-sm font-semibold text-gray-300">${formatCurrency(prod.costPrice)}</span>
                    </div>
                    <div>
                        <span class="text-xs text-gray-400 block">Preço de Venda</span>
                        <span class="text-sm font-semibold text-emerald-400">${formatCurrency(prod.sellPrice)}</span>
                    </div>
                </div>

                <div class="bg-dark-900/60 p-4 rounded-lg border border-dark-700 space-y-2">
                    <div class="flex justify-between text-xs text-gray-300">
                        <span>Lucro Unidade:</span>
                        <span class="font-bold text-blue-400">${formatCurrency(unitProfit)} (${marginPercent}%)</span>
                    </div>
                    <div class="flex justify-between text-xs text-gray-300">
                        <span>Total Vendido (Histórico):</span>
                        <span class="font-bold text-white">${prod.sold || 0} unidades</span>
                    </div>
                    <div class="flex justify-between text-xs text-gray-300">
                        <span>Faturamento deste Produto:</span>
                        <span class="font-bold text-emerald-400">${formatCurrency(totalRevenue)}</span>
                    </div>
                    <div class="flex justify-between text-xs text-gray-300 pt-2 border-t border-dark-700">
                        <span>Lucro Gerado:</span>
                        <span class="font-bold text-emerald-400">${formatCurrency(totalProfit)}</span>
                    </div>
                </div>

                <div class="flex gap-2">
                    <button onclick="closeModal('detailModal'); editProduct('${prod.id}')" class="flex-1 bg-amber-500 hover:bg-amber-600 text-dark-900 font-semibold py-2 rounded-lg text-sm transition-colors text-center">
                        <i class="fa-solid fa-pen mr-1"></i> Editar Dados
                    </button>
                </div>
            `;

            document.getElementById('btnDeleteFromDetail').onclick = () => {
                closeModal('detailModal');
                deleteProduct(prod.id);
            };

            openModal('detailModal');
        }

        // Popular Dropdown da Venda Rápida
        function populateSaleSelect() {
            const select = document.getElementById('saleSelectProduct');
            const availableProducts = products.filter(p => p.stock > 0);

            if (availableProducts.length === 0) {
                select.innerHTML = `<option value="">Nenhum produto em estoque</option>`;
                document.getElementById('saleTotalDisplay').innerText = 'R$ 0,00';
                return;
            }

            select.innerHTML = availableProducts.map(p => `
                <option value="${p.id}" data-price="${p.sellPrice}" data-stock="${p.stock}">
                    ${p.name} (Em estoque: ${p.stock}) - ${formatCurrency(p.sellPrice)}
                </option>
            `).join('');

            updateSalePriceInfo();
        }

        // Atualizar Preço Total na Tela de Venda Rápida
        function updateSalePriceInfo() {
            const select = document.getElementById('saleSelectProduct');
            const selectedOption = select.options[select.selectedIndex];
            
            if (!selectedOption || !selectedOption.value) {
                document.getElementById('saleTotalDisplay').innerText = 'R$ 0,00';
                return;
            }

            const price = parseFloat(selectedOption.getAttribute('data-price')) || 0;
            const maxStock = parseInt(selectedOption.getAttribute('data-stock')) || 0;
            
            const qtyInput = document.getElementById('saleQuantity');
            let qty = parseInt(qtyInput.value) || 1;

            if (qty > maxStock) {
                qty = maxStock;
                qtyInput.value = maxStock;
            }

            const total = price * qty;
            document.getElementById('saleTotalDisplay').innerText = formatCurrency(total);
        }

        // Registrar Venda pelo Modal
        function handleQuickSale(e) {
            e.preventDefault();
            const select = document.getElementById('saleSelectProduct');
            const id = select.value;
            const qty = parseInt(document.getElementById('saleQuantity').value) || 1;

            if (!id) return;

            const index = products.findIndex(p => p.id === id);
            if (index !== -1) {
                if (products[index].stock < qty) {
                    alert('Quantidade indisponível em estoque!');
                    return;
                }

                products[index].stock -= qty;
                products[index].sold = (products[index].sold || 0) + qty;

                recordSaleTransaction(products[index], qty);

                saveProducts();
                renderTable();
                updateDashboard();
                closeModal('saleModal');
            }
        }

        // Registrar Transação de Venda
        function recordSaleTransaction(product, quantity) {
            const sale = {
                id: Date.now().toString(),
                productId: product.id,
                productName: product.name,
                quantity: quantity,
                unitPrice: product.sellPrice,
                totalPrice: product.sellPrice * quantity,
                date: new Date().toLocaleString('pt-BR')
            };
            salesHistory.unshift(sale);
            saveSalesHistory();
        }

        // Renderizar Histórico de Vendas
        function renderSalesHistory() {
            const container = document.getElementById('salesHistoryList');
            if (salesHistory.length === 0) {
                container.innerHTML = `
                    <div class="text-center py-8 text-gray-500">
                        <i class="fa-solid fa-receipt text-3xl mb-2 block"></i>
                        Nenhuma venda registrada até o momento.
                    </div>
                `;
                return;
            }

            container.innerHTML = salesHistory.map(sale => `
                <div class="bg-dark-900/60 border border-dark-700 p-3 rounded-lg flex items-center justify-between">
                    <div>
                        <div class="font-medium text-white text-sm">${sale.productName}</div>
                        <div class="text-xs text-gray-400">${sale.date} • ${sale.quantity}x ${formatCurrency(sale.unitPrice)}</div>
                    </div>
                    <div class="text-right">
                        <div class="font-bold text-emerald-400 text-sm">${formatCurrency(sale.totalPrice)}</div>
                        <button onclick="revertSale('${sale.id}')" class="text-xs text-red-400 hover:underline mt-0.5">Estornar Venda</button>
                    </div>
                </div>
            `).join('');
        }

        // Estornar / Cancelar Venda do Histórico
        function revertSale(saleId) {
            const index = salesHistory.findIndex(s => s.id === saleId);
            if (index === -1) return;

            const sale = salesHistory[index];
            if (confirm(`Deseja estornar a venda de ${sale.quantity}x "${sale.productName}"? O estoque será devolvido.`)) {
                // Devolve estoque
                const prodIndex = products.findIndex(p => p.id === sale.productId);
                if (prodIndex !== -1) {
                    products[prodIndex].stock += sale.quantity;
                    products[prodIndex].sold = Math.max(0, (products[prodIndex].sold || 0) - sale.quantity);
                    saveProducts();
                }

                // Remove histórico
                salesHistory.splice(index, 1);
                saveSalesHistory();

                renderSalesHistory();
                renderTable();
                updateDashboard();
            }
        }

        // Limpar todo histórico
        function clearSalesHistory() {
            if (confirm('Tem certeza que deseja apagar todo o histórico de vendas?')) {
                salesHistory = [];
                saveSalesHistory();
                renderSalesHistory();
            }
        }

        // Exportar Backup JSON
        function exportData() {
            const payload = {
                products,
                salesHistory,
                exportedAt: new Date().toISOString()
            };
            const dataStr = "data:text/json;charset=utf-8," + encodeURIComponent(JSON.stringify(payload, null, 2));
            const downloadAnchor = document.createElement('a');
            downloadAnchor.setAttribute("href", dataStr);
            downloadAnchor.setAttribute("download", `barberstock_backup_${Date.now()}.json`);
            document.body.appendChild(downloadAnchor);
            downloadAnchor.click();
            downloadAnchor.remove();
        }

        // Importar Backup JSON
        function importData(e) {
            const fileReader = new FileReader();
            fileReader.onload = function(event) {
                try {
                    const parsed = JSON.parse(event.target.result);
                    if (parsed.products && Array.isArray(parsed.products)) {
                        products = parsed.products;
                        salesHistory = parsed.salesHistory || [];
                        saveProducts();
                        saveSalesHistory();
                        renderTable();
                        updateDashboard();
                        alert('Dados importados com sucesso!');
                        closeModal('dataModal');
                    } else {
                        alert('Arquivo de backup inválido.');
                    }
                } catch (err) {
                    alert('Erro ao carregar o arquivo JSON.');
                }
            };
            if (e.target.files[0]) {
                fileReader.readAsText(e.target.files[0]);
            }
        }

        // Resetar para Dados Iniciais
        function resetToDefaultData() {
            if (confirm('Atenção: Todos os seus dados atuais serão substituídos pelos de demonstração. Deseja continuar?')) {
                products = initialProducts;
                salesHistory = [];
                saveProducts();
                saveSalesHistory();
                renderTable();
                updateDashboard();
                closeModal('dataModal');
            }
        }
    </script>
</body>
</html>