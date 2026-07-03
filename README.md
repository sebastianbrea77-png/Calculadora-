control de inventario 
inventario 
```react
import React, { useState, useMemo } from 'react';
import { 
  Plus, 
  Search, 
  Trash2, 
  Edit3, 
  X,
  Minus,
  Clock,
  ChevronDown,
  Image as ImageIcon,
  LayoutGrid,
  List as ListIcon,
  Maximize2,
  Zap,
  Cpu,
  ShieldCheck,
  Gauge
} from 'lucide-react';

const App = () => {
  const [view, setView] = useState('inventory'); 
  const [displayMode, setDisplayMode] = useState('list'); // 'list' or 'grid'
  const [searchTerm, setSearchTerm] = useState('');
  const [filterCategory, setFilterCategory] = useState('Todas');
  const [selectedImage, setSelectedImage] = useState(null);
  const [isModalOpen, setIsModalOpen] = useState(false);

  // --- DATOS INDUSTRIALES COMPLETOS ---
  const [inventory, setInventory] = useState([
    { 
      id: 1, 
      name: 'Variador PowerFlex 755', 
      category: 'Control', 
      price: 3850, 
      stock: 3, 
      minStock: 2, 
      supplier: 'Rockwell Automation', 
      lastUpdated: '16 MAY 2024',
      image: 'https://images.unsplash.com/photo-1558449028-b53a39d100fc?auto=format&fit=crop&q=80&w=800',
      description: 'Convertidor de frecuencia para motores de inducción CA y motores de imanes permanentes.'
    },
    { 
      id: 2, 
      name: 'PLC Simatic S7-1500', 
      category: 'Automatización', 
      price: 2100, 
      stock: 8, 
      minStock: 4, 
      supplier: 'Siemens Industrial', 
      lastUpdated: '15 MAY 2024',
      image: 'https://images.unsplash.com/photo-159742324403d-d19504ba1541?auto=format&fit=crop&q=80&w=800',
      description: 'Controlador modular de gama alta para aplicaciones de automatización complejas.'
    },
    { 
      id: 3, 
      name: 'Transformador 500kVA', 
      category: 'Potencia', 
      price: 12500, 
      stock: 1, 
      minStock: 1, 
      supplier: 'ABB Power', 
      lastUpdated: '14 MAY 2024',
      image: 'https://images.unsplash.com/photo-1621905235858-a8869f997637?auto=format&fit=crop&q=80&w=800',
      description: 'Transformador de distribución de tipo seco para interiores.'
    },
    { 
      id: 4, 
      name: 'Analizador PM8000', 
      category: 'Medición', 
      price: 1450, 
      stock: 6, 
      minStock: 2, 
      supplier: 'Schneider Electric', 
      lastUpdated: '12 MAY 2024',
      image: 'https://images.unsplash.com/photo-1581092334651-ddf26d9a1930?auto=format&fit=crop&q=80&w=800',
      description: 'Monitor de energía y potencia compacto para redes críticas.'
    },
    { 
      id: 5, 
      name: 'Disyuntor Vacío 12kV', 
      category: 'Protección', 
      price: 4200, 
      stock: 4, 
      minStock: 2, 
      supplier: 'Eaton Corp', 
      lastUpdated: '16 MAY 2024',
      image: 'https://images.unsplash.com/photo-1555664424-778a1e5e1b48?auto=format&fit=crop&q=80&w=800',
      description: 'Interruptor automático de vacío para media tensión.'
    },
    { 
      id: 6, 
      name: 'HMI Comfort 12"', 
      category: 'Interfaces', 
      price: 2900, 
      stock: 2, 
      minStock: 3, 
      supplier: 'Siemens Industrial', 
      lastUpdated: '16 MAY 2024',
      image: 'https://images.unsplash.com/photo-1518770660439-4636190af475?auto=format&fit=crop&q=80&w=800',
      description: 'Panel de operador con pantalla panorámica brillante de 16 millones de colores.'
    },
    { 
      id: 7, 
      name: 'Relé de Protección SEL', 
      category: 'Protección', 
      price: 1800, 
      stock: 12, 
      minStock: 5, 
      supplier: 'Schweitzer Engineering', 
      lastUpdated: '10 MAY 2024',
      image: 'https://images.unsplash.com/photo-1517420812314-8b1711739499?auto=format&fit=crop&q=80&w=800',
      description: 'Protección avanzada, control y monitoreo de sistemas eléctricos.'
    },
    { 
      id: 8, 
      name: 'Actuador Eléctrico', 
      category: 'Control', 
      price: 950, 
      stock: 0, 
      minStock: 2, 
      supplier: 'Rotork PLC', 
      lastUpdated: '08 MAY 2024',
      image: 'https://images.unsplash.com/photo-1581092160562-40aa08e78837?auto=format&fit=crop&q=80&w=800',
      description: 'Control de válvulas de alta precisión para procesos industriales.'
    }
  ]);

  const stats = useMemo(() => {
    const totalValue = inventory.reduce((acc, item) => acc + (item.price * item.stock), 0);
    const lowStock = inventory.filter(item => item.stock <= item.minStock && item.stock > 0).length;
    return { totalValue, lowStock, totalItems: inventory.length };
  }, [inventory]);

  const categories = ['Todas', ...new Set(inventory.map(item => item.category))];

  const filteredInventory = inventory.filter(item => {
    const matchesSearch = item.name.toLowerCase().includes(searchTerm.toLowerCase());
    const matchesCategory = filterCategory === 'Todas' || item.category === filterCategory;
    return matchesSearch && matchesCategory;
  });

  return (
    <div className="min-h-screen bg-[#FDFCFB] text-[#1A1A1A] font-serif p-6 md:p-12 transition-all">
      <div className="max-w-7xl mx-auto">
        
        {/* Header Superior */}
        <header className="flex flex-col md:flex-row justify-between items-end mb-16 border-b border-[#E5E1DA] pb-10">
          <div>
            <h2 className="text-[10px] tracking-[0.6em] font-sans font-bold text-[#A69F92] mb-5 uppercase italic">Asset Management & Visual Archive</h2>
            <h1 className="text-5xl md:text-7xl font-light tracking-tighter text-[#1A1A1A]">Archives Électriques</h1>
          </div>
          
          <nav className="flex gap-10 mt-10 md:mt-0 font-sans text-[11px] font-bold tracking-[0.25em] uppercase text-[#A69F92]">
            <button onClick={() => setView('inventory')} className={`hover:text-[#1A1A1A] transition-all pb-1 ${view === 'inventory' ? 'text-[#1A1A1A] border-b-2 border-[#1A1A1A]' : ''}`}>Inventaire</button>
            <button onClick={() => setView('reports')} className={`hover:text-[#1A1A1A] transition-all pb-1 ${view === 'reports' ? 'text-[#1A1A1A] border-b-2 border-[#1A1A1A]' : ''}`}>Analytique</button>
          </nav>
        </header>

        {view === 'inventory' && (
          <div className="animate-in fade-in duration-1000">
            {/* Controles de Vista */}
            <div className="flex flex-col lg:flex-row justify-between items-center mb-12 gap-10">
              <div className="flex items-center gap-10">
                <div className="relative group">
                  <Search className="absolute left-0 top-1/2 -translate-y-1/2 text-[#A69F92]" size={16} strokeWidth={1.5} />
                  <input 
                    type="text" 
                    placeholder="Filtrer par spécification..." 
                    className="w-64 pl-8 pr-4 py-2 bg-transparent border-b border-[#E5E1DA] focus:border-[#1A1A1A] outline-none transition-all font-sans text-xs tracking-wider"
                    value={searchTerm}
                    onChange={(e) => setSearchTerm(e.target.value)}
                  />
                </div>
                
                <div className="flex items-center gap-2 border border-[#E5E1DA] rounded-full p-1">
                  <button 
                    onClick={() => setDisplayMode('list')}
                    className={`p-2 rounded-full transition-all ${displayMode === 'list' ? 'bg-[#1A1A1A] text-white' : 'text-[#A69F92] hover:text-[#1A1A1A]'}`}
                  >
                    <ListIcon size={14} />
                  </button>
                  <button 
                    onClick={() => setDisplayMode('grid')}
                    className={`p-2 rounded-full transition-all ${displayMode === 'grid' ? 'bg-[#1A1A1A] text-white' : 'text-[#A69F92] hover:text-[#1A1A1A]'}`}
                  >
                    <LayoutGrid size={14} />
                  </button>
                </div>
              </div>

              <div className="flex items-center gap-10">
                <select 
                  className="bg-transparent border-none font-sans text-[10px] font-bold tracking-widest uppercase text-[#A69F92] outline-none cursor-pointer"
                  value={filterCategory}
                  onChange={(e) => setFilterCategory(e.target.value)}
                >
                  {categories.map(cat => <option key={cat} value={cat}>{cat}</option>)}
                </select>
                <button 
                  onClick={() => setIsModalOpen(true)}
                  className="bg-[#1A1A1A] text-[#FDFCFB] px-10 py-4 rounded-[2px] font-sans text-[10px] font-bold tracking-[0.2em] uppercase hover:bg-[#333333] transition-all flex items-center gap-3 shadow-md"
                >
                  <Plus size={14} /> Nouveau Produit
                </button>
              </div>
            </div>

            {/* Renderizado Condicional de Vista */}
            {displayMode === 'list' ? (
              <div className="space-y-6">
                {filteredInventory.map((item) => (
                  <div key={item.id} className="group flex flex-col md:flex-row md:items-center p-8 border border-[#E5E1DA] bg-white hover:border-[#1A1A1A] transition-all duration-700">
                    <div 
                      className="w-24 h-24 bg-[#F9F7F5] overflow-hidden flex-shrink-0 cursor-zoom-in relative mb-6 md:mb-0 md:mr-10"
                      onClick={() => setSelectedImage(item)}
                    >
                      <img src={item.image} className="w-full h-full object-cover transition-transform duration-700 group-hover:scale-110" />
                      <div className="absolute inset-0 bg-black/0 group-hover:bg-black/5 flex items-center justify-center opacity-0 group-hover:opacity-100 transition-all">
                        <Maximize2 size={16} className="text-white" />
                      </div>
                    </div>
                    
                    <div className="flex-1">
                      <span className="text-[9px] font-sans font-black tracking-widest uppercase text-[#A69F92] mb-1 block">{item.category}</span>
                      <h3 className="text-2xl font-light">{item.name}</h3>
                      <p className="text-[10px] font-sans text-[#A69F92] tracking-widest uppercase mt-1">{item.supplier}</p>
                    </div>

                    <div className="flex items-center justify-between md:justify-end gap-16 mt-6 md:mt-0">
                      <div className="text-right">
                        <p className="text-[9px] font-sans font-bold tracking-[0.2em] text-[#A69F92] mb-1 uppercase">Prix</p>
                        <p className="text-xl font-light">${item.price.toLocaleString()}</p>
                      </div>
                      <div className="text-center px-10 border-x border-[#E5E1DA]">
                        <p className={`text-2xl font-light ${item.stock === 0 ? 'text-red-300' : ''}`}>{item.stock}</p>
                        <p className="text-[9px] font-sans text-[#A69F92] uppercase tracking-widest">Unités</p>
                      </div>
                      <div className="flex gap-4 opacity-0 group-hover:opacity-100 transition-all">
                        <button className="text-[#A69F92] hover:text-[#1A1A1A]"><Edit3 size={16}/></button>
                        <button className="text-[#A69F92] hover:text-red-800"><Trash2 size={16}/></button>
                      </div>
                    </div>
                  </div>
                ))}
              </div>
            ) : (
              <div className="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-10">
                {filteredInventory.map((item) => (
                  <div key={item.id} className="group animate-in slide-in-from-bottom-5 duration-700">
                    <div 
                      className="aspect-square bg-[#F9F7F5] overflow-hidden mb-6 relative cursor-zoom-in border border-[#E5E1DA] group-hover:border-[#1A1A1A] transition-all"
                      onClick={() => setSelectedImage(item)}
                    >
                      <img src={item.image} className="w-full h-full object-cover transition-transform duration-1000 group-hover:scale-105" />
                      <div className="absolute top-4 right-4 bg-white/90 backdrop-blur px-3 py-1 text-[10px] font-sans font-bold tracking-widest uppercase">
                        ${item.price.toLocaleString()}
                      </div>
                      <div className="absolute inset-0 bg-black/0 group-hover:bg-black/10 transition-all"></div>
                    </div>
                    <div className="px-1">
                      <div className="flex justify-between items-start mb-2">
                        <h3 className="text-lg font-light tracking-tight">{item.name}</h3>
                        <span className={`text-xs font-sans font-bold ${item.stock <= item.minStock ? 'text-[#C4A484]' : 'text-[#A69F92]'}`}>
                          {item.stock} u.
                        </span>
                      </div>
                      <p className="text-[9px] font-sans uppercase tracking-[0.2em] text-[#A69F92]">{item.category} — {item.supplier}</p>
                    </div>
                  </div>
                ))}
              </div>
            )}
          </div>
        )}

        {view === 'reports' && (
          <div className="animate-in fade-in duration-1000 py-10">
            <div className="grid grid-cols-1 md:grid-cols-3 gap-20">
              <div className="md:col-span-2 bg-[#1A1A1A] p-20 text-white flex flex-col justify-between aspect-video md:aspect-auto">
                <h3 className="text-[11px] font-sans font-bold tracking-[0.5em] uppercase text-[#A69F92]">Valeur Totale du Stock</h3>
                <p className="text-8xl font-light tracking-tighter my-10 animate-pulse">${stats.totalValue.toLocaleString()}</p>
                <div className="flex gap-10 border-t border-white/10 pt-10">
                  <div className="flex items-center gap-3">
                    <ShieldCheck size={16} className="text-[#A69F92]" />
                    <span className="text-[9px] font-sans uppercase tracking-widest">Actifs Assurés</span>
                  </div>
                  <div className="flex items-center gap-3">
                    <Gauge size={16} className="text-[#A69F92]" />
                    <span className="text-[9px] font-sans uppercase tracking-widest">Précision 99.8%</span>
                  </div>
                </div>
              </div>
              <div className="space-y-12 flex flex-col justify-center">
                <div className="text-center md:text-left">
                  <p className="text-[10px] font-sans font-bold tracking-widest uppercase text-[#A69F92] mb-4">État des Infrastructures</p>
                  <p className="text-4xl font-light tracking-tight italic">"L'ordre visuel précède l'efficacité industrielle."</p>
                </div>
                <div className="border-t border-[#E5E1DA] pt-10">
                   <div className="flex justify-between mb-4">
                     <span className="text-[10px] font-sans font-bold uppercase tracking-widest">Contrôle Automatisé</span>
                     <span className="text-[10px] font-sans font-bold">85%</span>
                   </div>
                   <div className="h-[1px] bg-[#E5E1DA] w-full">
                     <div className="h-[1px] bg-[#1A1A1A] w-[85%] transition-all duration-1000"></div>
                   </div>
                </div>
              </div>
            </div>
          </div>
        )}
      </div>

      {/* Visor de Imágenes a Pantalla Completa */}
      {selectedImage && (
        <div className="fixed inset-0 z-50 flex items-center justify-center p-4 md:p-10 animate-in fade-in duration-500">
          <div className="absolute inset-0 bg-[#FDFCFB]/98 backdrop-blur-md" onClick={() => setSelectedImage(null)}></div>
          
          <div className="relative w-full max-w-6xl bg-white shadow-2xl flex flex-col md:flex-row overflow-hidden rounded-[2px] animate-in zoom-in-95 duration-500">
            <button 
              onClick={() => setSelectedImage(null)}
              className="absolute top-6 right-6 z-10 text-[#A69F92] hover:text-[#1A1A1A] transition-colors"
            >
              <X size={24} strokeWidth={1} />
            </button>
            
            <div className="w-full md:w-2/3 aspect-square md:aspect-auto bg-[#F9F7F5]">
              <img src={selectedImage.image} className="w-full h-full object-cover" alt={selectedImage.name} />
            </div>
            
            <div className="w-full md:w-1/3 p-12 flex flex-col justify-center">
              <span className="text-[10px] font-sans font-black tracking-[0.4em] uppercase text-[#A69F92] mb-6">{selectedImage.category}</span>
              <h2 className="text-4xl font-light tracking-tighter mb-4">{selectedImage.name}</h2>
              <p className="text-[11px] font-sans font-bold tracking-[0.2em] uppercase text-[#1A1A1A] mb-8">{selectedImage.supplier}</p>
              
              <div className="w-12 h-[1px] bg-[#E5E1DA] mb-8"></div>
              
              <p className="text-lg italic text-[#A69F92] leading-relaxed mb-10">
                {selectedImage.description}
              </p>
              
              <div className="grid grid-cols-2 gap-10">
                <div>
                  <p className="text-[9px] font-sans font-bold tracking-widest text-[#A69F92] uppercase mb-2">Prix Unitaire</p>
                  <p className="text-2xl font-light">${selectedImage.price.toLocaleString()}</p>
                </div>
                <div>
                  <p className="text-[9px] font-sans font-bold tracking-widest text-[#A69F92] uppercase mb-2">Disponibilité</p>
                  <p className="text-2xl font-light">{selectedImage.stock} Unités</p>
                </div>
              </div>
              
              <button className="mt-12 w-full py-4 bg-[#1A1A1A] text-white font-sans text-[10px] font-bold tracking-[0.3em] uppercase hover:bg-[#333333] transition-all">
                Générer Rapport Technique
              </button>
            </div>
          </div>
        </div>
      )}

      {/* Modal Genérico */}
      {isModalOpen && (
        <div className="fixed inset-0 z-50 flex items-center justify-center p-6 animate-in fade-in duration-300">
           <div className="absolute inset-0 bg-[#FDFCFB]/90 backdrop-blur-sm" onClick={() => setIsModalOpen(false)}></div>
           <div className="relative bg-white p-16 border border-[#E5E1DA] max-w-lg w-full text-center shadow-xl">
             <h2 className="text-3xl font-light tracking-tight mb-6">Nouveau SK-Unit</h2>
             <p className="text-[#A69F92] mb-10 italic">L'accès à la base de données est restreint en mode consultation.</p>
             <button onClick={() => setIsModalOpen(false)} className="px-12 py-3 bg-[#1A1A1A] text-white font-sans text-[10px] font-bold tracking-widest uppercase">Fermer</button>
           </div>
        </div>
      )}
    </div>
  );
};

export default App;

```
