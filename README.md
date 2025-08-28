import React, { useState, useEffect, createContext, useContext } from 'react';
import { LogIn, LayoutDashboard, Briefcase, Users, Settings, Wrench, Handshake, DollarSign, List, Truck, BarChart, ShoppingBag, FolderOpen, UserCheck, Zap, Package, ShoppingCart, FileText, MessageCircle, BarChart2, Star, ListChecks, Menu, X, Bot, Plus, Edit, Trash2, Search, Info } from 'lucide-react';

// Create a context to manage user authentication state
const AuthContext = createContext(null);

// Mock data for users and their roles
const mockUsers = [
  { username: 'ceo', password: 'ceo', role: 'CEO' },
  { username: 'osm1', password: 'osm1', role: 'OSM' },
  { username: 'osm2', password: 'osm2', role: 'OSM' },
  { username: 'cm', password: 'cm', role: 'CM' },
  { username: 'pscm', password: 'pscm', role: 'PSCM' },
  { username: 'afm', password: 'afm', role: 'AFM' },
  { username: 'hrm', password: 'hrm', role: 'HRM' },
  { username: 'sysadmin', password: 'sysadmin', role: 'Sysadmin' },
  // Jukumu jipya limeongezwa: SEA
  { username: 'sea1', password: 'sea1', role: 'SEA' },
  { username: 'sea2', password: 'sea2', role: 'SEA' },
];

// Mock data for tasks
const mockTasks = [
  { id: 1, title: 'Kagua ripoti za mauzo za kila mwezi', department: 'OSM', status: 'Inasubiri', assignedBy: 'CEO' },
  { id: 2, title: 'Fuatilia utekelezaji wa sera mpya', department: 'CM', status: 'Inaendelea', assignedBy: 'CEO' },
  { id: 3, title: 'Nunua vifaa vipya vya ofisi', department: 'PSCM', status: 'Imekamilika', assignedBy: 'OSM' },
  { id: 4, title: 'Kamilisha mshahara wa wafanyakazi wa mwezi', department: 'AFM', status: 'Inasubiri', assignedBy: 'HRM' },
  { id: 5, title: 'Panga mafunzo ya wafanyakazi wapya', department: 'HRM', status: 'Inaendelea', assignedBy: 'CEO' },
  { id: 6, title: 'Fuatilia na urekebishe masuala ya mtandao', department: 'Sysadmin', status: 'Imekamilika', assignedBy: 'CEO' },
  { id: 7, title: 'Unda ripoti ya bajeti', department: 'AFM', status: 'Inasubiri', assignedBy: 'CEO' },
  { id: 8, title: 'Tafiti wauzaji wapya', department: 'PSCM', status: 'Inaendelea', assignedBy: 'PSCM' },
  { id: 9, title: 'Kagua utendaji wa mauzo wa robo mwaka', department: 'OSM', status: 'Inaendelea', assignedBy: 'CEO' },
  { id: 10, title: 'Tathmini ya wafanyakazi', department: 'HRM', status: 'Inasubiri', assignedBy: 'HRM' },
  // Kazi mpya za SEA zimeongezwa
  { id: 11, title: 'Kamilisha mauzo ya kila siku', department: 'SEA', status: 'Inaendelea', assignedBy: 'OSM' },
  { id: 12, title: 'Kagua orodha ya bidhaa zilizopo', department: 'SEA', status: 'Imekamilika', assignedBy: 'OSM' },
  { id: 13, title: 'Panga bidhaa mpya kwenye rafu', department: 'SEA', status: 'Inasubiri', assignedBy: 'CM' },
];

// App component
const App = () => {
  const [user, setUser] = useState(null);
  const [currentPage, setCurrentPage] = useState('login'); // Hali mpya ya kufuatilia ukurasa

  const login = (username, password) => {
    const foundUser = mockUsers.find(
      (u) => u.username === username && u.password === password
    );
    if (foundUser) {
      setUser(foundUser);
      setCurrentPage('dashboard'); // Nenda kwenye dashibodi baada ya kuingia
    } else {
      alert('Jina la mtumiaji au nenosiri si sahihi.');
    }
  };

  const logout = () => {
    setUser(null);
    setCurrentPage('login'); // Rudi kwenye ukurasa wa kuingia baada ya kutoka
  };

  const navigateTo = (page) => {
    setCurrentPage(page);
  };

  return (
    <AuthContext.Provider value={{ user, login, logout, navigateTo }}>
      <div className="min-h-screen bg-white p-4">
        <div className="max-w-7xl mx-auto shadow-lg rounded-xl p-6 md:p-8" style={{ backgroundColor: '#234251cf' }}>
          <Header user={user} currentPage={currentPage} />
          {currentPage === 'login' && !user && <Login />}
          {currentPage === 'register' && <Register />}
          {user && currentPage === 'dashboard' && <Dashboard />}
        </div>
      </div>
    </AuthContext.Provider>
  );
};

// Header component with conditional rendering
const Header = ({ user, currentPage }) => {
  const { logout, navigateTo } = useContext(AuthContext);

  return (
    <header className="flex justify-between items-center pb-6 mb-6 border-b-2 border-gray-200">
      {/* Muundo wa "SMS" umehama kushoto */}
      <div className="flex flex-col items-center justify-center">
            {/* Custom CSS for the silver and gold metallic text with improved contrast */}
            <style>{`
                .metallic-gold-text {
                    background: linear-gradient(145deg, #FFD700 0%, #FFA500 25%, #FFC700 50%, #FFA500 75%, #FFD700 100%);
                    -webkit-background-clip: text;
                    -webkit-text-fill-color: transparent;
                    filter: drop-shadow(0 2px 2px rgba(0,0,0,0.2));
                    font-weight: bold;
                }
            `}</style>
            <h1 className="metallic-gold-text text-3xl sm:text-4xl md:text-5xl tracking-tight">SMS</h1>
            <p className="text-sm sm:text-base md:text-lg text-white font-light mt-1 text-center">Shops Management System</p>
        </div>

      {user ? (
        <div className="flex items-center space-x-4">
          <span className="text-white font-medium hidden md:block">Umeingia kama: <span className="font-bold text-amber-500">{user.role}</span></span>
          <button
            onClick={logout}
            className="flex items-center space-x-2 px-4 py-2 bg-red-500 text-white rounded-lg shadow-md hover:bg-red-600 transition-all duration-300"
          >
            <LogIn size={20} />
            <span>Toka</span>
          </button>
        </div>
      ) : (
        /* Kitufe cha "Jisajili nasi" sasa kinafanya kazi ya kusogea kwenye ukurasa wa usajili */
        <div className="flex items-center space-x-4">
          {currentPage === 'register' && (
            <button
                onClick={() => navigateTo('login')}
                className="flex items-center space-x-2 px-4 py-2 bg-gray-500 text-white rounded-lg shadow-md hover:bg-gray-600 transition-all duration-300"
            >
                <LogIn size={20} />
                <span>Ingia</span>
            </button>
          )}
          {currentPage === 'login' && (
            <button
                onClick={() => navigateTo('register')}
                className="flex items-center space-x-2 px-4 py-2 bg-amber-500 text-white rounded-lg shadow-md hover:bg-amber-600 transition-all duration-300"
            >
                <span>Jisajili nasi</span>
            </button>
          )}
        </div>
      )}
    </header>
  );
};

// Login page component
const Login = () => {
  const { login } = useContext(AuthContext);
  const [username, setUsername] = useState('');
  const [password, setPassword] = useState('');

  const handleSubmit = (e) => {
    e.preventDefault();
    login(username, password);
  };

  return (
    <>
      {/* Custom CSS for the silver placeholder text */}
      <style>{`
        .silver-placeholder::placeholder {
          color: #C0C0C0;
        }
      `}</style>
      <div className="flex flex-col items-center justify-center min-h-[50vh]">
        <form onSubmit={handleSubmit} className="p-8 rounded-xl shadow-lg w-full max-w-sm" style={{ backgroundColor: '#2a4957' }}>
          {/* Kichwa cha "Ingia" kimekuwa cha dhahabu */}
          <h2 className="text-2xl font-bold text-center mb-6 text-amber-500">Ingia</h2>
          <div className="space-y-4">
            <div>
              {/* Lebo ya "Jina la Mtumiaji" imekuwa ya dhahabu */}
              <label className="block text-amber-500 font-medium mb-1">Jina la Mtumiaji</label>
              <input
                type="text"
                value={username}
                onChange={(e) => setUsername(e.target.value)}
                className="w-full px-4 py-2 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-amber-500 silver-placeholder text-gray-800"
                placeholder="Ingiza jina la mtumiaji"
              />
            </div>
            <div>
              {/* Lebo ya "Nenosiri" imekuwa ya dhahabu */}
              <label className="block text-amber-500 font-medium mb-1">Nenosiri</label>
              <input
                type="password"
                value={password}
                onChange={(e) => setPassword(e.target.value)}
                className="w-full px-4 py-2 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-amber-500 silver-placeholder text-gray-800"
                placeholder="Ingiza nenosiri"
              />
            </div>
            {/* Rangi ya dhahabu (amber) imetumika kwa kitufe cha kuingia */}
            <button
              type="submit"
              className="w-full bg-amber-500 text-white py-3 rounded-lg font-semibold shadow-md hover:bg-amber-600 transition-all duration-300"
            >
              Ingia
            </button>
            {/* Linki ya 'Umesahau nenosiri' imeongezwa hapa chini */}
            <div className="text-center mt-4">
              <a href="#" className="text-amber-500 hover:text-amber-600 font-medium transition-colors duration-200">
                Je, umesahau nenosiri?
              </a>
            </div>
          </div>
        </form>
      </div>
    </>
  );
};

// Main dashboard component that renders based on user role
const Dashboard = () => {
  const { user } = useContext(AuthContext);

  if (!user) {
    return <Login />;
  }

  switch (user.role) {
    case 'CEO':
      return <CEODashboard />;
    case 'Sysadmin':
      return <SysadminPanel />;
    case 'SEA':
      return <SEADashboard />;
    default:
      return <DepartmentDashboard department={user.role} />;
  }
};

// Reusable Stat Card component
const StatCard = ({ title, value, icon, color, textColor }) => (
  <div className={`p-6 rounded-xl shadow-md flex items-center justify-between ${color}`}>
    <div>
      <p className={`text-sm font-medium ${textColor}`}>{title}</p>
      <p className="text-3xl font-bold text-gray-800">{value}</p>
    </div>
    <div className={`p-3 rounded-full ${color}`}>
      {React.cloneElement(icon, { size: 36, className: textColor })}
    </div>
  </div>
);

// Reusable Status Badge component
const StatusBadge = ({ status }) => {
  let colorClass = '';
  switch (status) {
    case 'Imekamilika':
      colorClass = 'bg-green-100 text-green-800';
      break;
    case 'Inaendelea':
      colorClass = 'bg-yellow-100 text-yellow-800';
      break;
    case 'Inasubiri':
      colorClass = 'bg-red-100 text-red-800';
      break;
    default:
      colorClass = 'bg-gray-100 text-gray-800';
  }
  return (
    <span className={`inline-flex items-center px-3 py-1.5 rounded-full text-xs font-semibold ${colorClass}`}>
      {status}
    </span>
  );
};

// CEO's dashboard component
const CEODashboard = () => {
  const totalTasks = mockTasks.length;
  const completedTasks = mockTasks.filter(t => t.status === 'Imekamilika').length;
  const inProgressTasks = mockTasks.filter(t => t.status === 'Inaendelea').length;
  const pendingTasks = mockTasks.filter(t => t.status === 'Inasubiri').length;

  return (
    <div className="p-4">
      <h2 className="text-3xl font-bold mb-6 text-gray-100 flex items-center space-x-3">
        <LayoutDashboard size={30} className="text-indigo-600" />
        <span>Dashibodi ya Mkurugenzi Mkuu</span>
      </h2>
      <div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-6 mb-8">
        <StatCard title="Jumla ya Kazi" value={totalTasks} icon={<List />} color="bg-indigo-100" textColor="text-indigo-600" />
        <StatCard title="Kazi Zilizokamilika" value={completedTasks} icon={<UserCheck />} color="bg-green-100" textColor="text-green-600" />
        <StatCard title="Kazi Zinazoendelea" value={inProgressTasks} icon={<Zap />} color="bg-yellow-100" textColor="text-yellow-600" />
        <StatCard title="Kazi Zilizosubiriwa" value={pendingTasks} icon={<Wrench />} color="bg-red-100" textColor="text-red-600" />
      </div>

      <div className="bg-gray-50 rounded-xl p-6 shadow-md">
        <h3 className="text-xl font-bold mb-4 text-gray-800 flex items-center space-x-2">
          <FolderOpen size={24} className="text-indigo-600" />
          <span>Utekelezaji wa Majukumu na Idara</span>
        </h3>
        <table className="min-w-full bg-white rounded-xl overflow-hidden shadow-sm">
          <thead>
            <tr className="bg-gray-200 text-left text-gray-600 font-semibold uppercase text-sm">
              <th className="py-3 px-6">Kazi</th>
              <th className="py-3 px-6">Idara</th>
              <th className="py-3 px-6">Hali</th>
              <th className="py-3 px-6">Imepangwa na</th>
            </tr>
          </thead>
          <tbody>
            {mockTasks.map(task => (
              <tr key={task.id} className="border-t border-gray-200 hover:bg-gray-50 transition-colors duration-200">
                <td className="py-4 px-6 font-medium text-gray-700">{task.title}</td>
                <td className="py-4 px-6 text-gray-600">{task.department}</td>
                <td className="py-4 px-6">
                  <StatusBadge status={task.status} />
                </td>
                <td className="py-4 px-6 text-gray-600">{task.assignedBy}</td>
              </tr>
            ))}
          </tbody>
        </table>
      </div>
    </div>
  );
};

// Department Dashboard component
const DepartmentDashboard = ({ department }) => {
  const departmentTasks = mockTasks.filter(t => t.department === department);

  const getIconForDepartment = (dept) => {
    switch(dept) {
      case 'OSM': return <ShoppingBag />;
      case 'CM': return <Handshake />;
      case 'PSCM': return <Truck />;
      case 'AFM': return <DollarSign />;
      case 'HRM': return <Users />;
      default: return <Briefcase />;
    }
  };

  return (
    <div className="p-4">
      <h2 className="text-3xl font-bold mb-6 text-gray-100 flex items-center space-x-3">
        {getIconForDepartment(department)}
        <span>Dashibodi ya Idara ya {department}</span>
      </h2>
      <div className="bg-gray-50 rounded-xl p-6 shadow-md">
        <h3 className="text-xl font-bold mb-4 text-gray-800 flex items-center space-x-2">
          <List size={24} className="text-indigo-600" />
          <span>Majukumu Yako</span>
        </h3>
        {departmentTasks.length > 0 ? (
          <table className="min-w-full bg-white rounded-xl overflow-hidden shadow-sm">
            <thead>
              <tr className="bg-gray-200 text-left text-gray-600 font-semibold uppercase text-sm">
                <th className="py-3 px-6">Kazi</th>
                <th className="py-3 px-6">Hali</th>
                <th className="py-3 px-6">Imepangwa na</th>
              </tr>
            </thead>
            <tbody>
              {departmentTasks.map(task => (
                <tr key={task.id} className="border-t border-gray-200 hover:bg-gray-50 transition-colors duration-200">
                  <td className="py-4 px-6 font-medium text-gray-700">{task.title}</td>
                  <td className="py-4 px-6">
                    <StatusBadge status={task.status} />
                  </td>
                  <td className="py-4 px-6 text-gray-600">{task.assignedBy}</td>
                </tr>
              ))}
            </tbody>
          </table>
        ) : (
          <p className="text-gray-500 italic">Hakuna majukumu yaliyopangwa kwa idara hii kwa sasa.</p>
        )}
      </div>
    </div>
  );
};

// New SEA Dashboard component with a sidebar
const SEADashboard = () => {
  const [activeMenu, setActiveMenu] = useState('Dashibodi ya Mauzo');
  const [isMenuOpen, setIsMenuOpen] = useState(false);

  // Menu items and their corresponding icons
  const menuItems = [
    { name: 'Dashibodi ya Mauzo', icon: <BarChart2 size={20} /> },
    { name: 'Bidhaa', icon: <Package size={20} /> },
    { name: 'Mauzo', icon: <ShoppingCart size={20} /> },
    { name: 'Oda za Bidhaa', icon: <ListChecks size={20} /> },
    { name: 'Maoni ya Wateja', icon: <Star size={20} /> },
    { name: 'Ujumbe & Arifa', icon: <MessageCircle size={20} /> },
    { name: 'Rasimu za Mauzo', icon: <FileText size={20} /> },
    { name: 'Ripoti za Mauzo', icon: <BarChart2 size={20} /> },
  ];

  // Render content based on the active menu item
  const renderContent = () => {
    switch (activeMenu) {
      case 'Dashibodi ya Mauzo':
        const seaTasks = mockTasks.filter(t => t.department === 'SEA');
        return (
          <div className="p-4" style={{ backgroundColor: '#2a4957', color: '#fff' }}>
            <h3 className="text-2xl font-bold mb-4 text-gray-100 flex items-center space-x-2">
              <BarChart2 size={24} className="text-indigo-600" />
              <span>Dashibodi ya Mauzo</span>
            </h3>
            <p className="text-gray-200 mb-4">Hii ndio dashibodi yako kuu. Inaonyesha muhtasari wa utendaji wako.</p>
            <div className="bg-white rounded-xl p-6 shadow-md">
              <h4 className="text-xl font-bold mb-4">Majukumu Yangu</h4>
              {seaTasks.length > 0 ? (
                <table className="min-w-full bg-white rounded-xl overflow-hidden shadow-sm">
                  <thead>
                    <tr className="bg-gray-200 text-left text-gray-600 font-semibold uppercase text-sm">
                      <th className="py-3 px-6">Kazi</th>
                      <th className="py-3 px-6">Hali</th>
                      <th className="py-3 px-6">Imepangwa na</th>
                    </tr>
                  </thead>
                  <tbody>
                    {seaTasks.map(task => (
                      <tr key={task.id} className="border-t border-gray-200 hover:bg-gray-50 transition-colors duration-200">
                        <td className="py-4 px-6 font-medium text-gray-700">{task.title}</td>
                        <td className="py-4 px-6">
                          <StatusBadge status={task.status} />
                        </td>
                        <td className="py-4 px-6 text-gray-600">{task.assignedBy}</td>
                      </tr>
                    ))}
                  </tbody>
                </table>
              ) : (
                <p className="text-gray-500 italic">Hakuna majukumu yaliyopangwa kwa idara hii kwa sasa.</p>
              )}
            </div>
          </div>
        );
      case 'Bidhaa':
        // Hapa ndipo tunaita kipengele kipya cha Bidhaa
        return <ProductsFeature />;
      case 'Mauzo':
        return <PlaceholderContent title="Mauzo" description="Hapa unaweza kurekodi mauzo mapya." />;
      case 'Oda za Bidhaa':
        return <PlaceholderContent title="Oda za Bidhaa" description="Hapa utaona orodha ya oda za bidhaa zilizopo." />;
      case 'Maoni ya Wateja':
        return <CustomerFeedbackFeature />; // Tumia sehemu mpya ya "Maoni ya Wateja"
      case 'Ujumbe & Arifa':
        return <PlaceholderContent title="Ujumbe & Arifa" description="Hapa utaona ujumbe na arifa mpya." />;
      case 'Rasimu za Mauzo':
        return <PlaceholderContent title="Rasimu za Mauzo" description="Hapa unaweza kusimamia rasimu za mauzo ambazo hazijakamilika." />;
      case 'Ripoti za Mauzo':
        return <PlaceholderContent title="Ripoti za Mauzo" description="Hapa unaweza kutengeneza na kuona ripoti za utendaji wako." />;
      default:
        return null;
    }
  };

  return (
    <div className="flex flex-col md:flex-row">
      {/* Hamburger Menu Button (visible on mobile) */}
      <div className="md:hidden flex justify-end mb-4">
        <button
          onClick={() => setIsMenuOpen(!isMenuOpen)}
          className="p-2 text-gray-200 rounded-lg hover:bg-gray-700 transition-colors"
        >
          <Menu size={28} />
        </button>
      </div>

      {/* Sidebar (visible on desktop or when menu is open on mobile) */}
      <div
        className={`w-full md:w-64 bg-gray-50 rounded-xl p-4 md:mr-4 shadow-md md:mb-0 transform transition-all duration-300 ease-in-out ${
          isMenuOpen ? 'block' : 'hidden md:block'
        }`}
      >
        <h3 className="text-lg font-bold mb-4 text-gray-800">Menyu</h3>
        <nav>
          <ul>
            {menuItems.map(item => (
              <li key={item.name} className="mb-2">
                <button
                  onClick={() => {
                    setActiveMenu(item.name);
                    setIsMenuOpen(false); // Close menu on click
                  }}
                  className={`flex items-center space-x-3 w-full text-left px-4 py-3 rounded-lg transition-colors duration-200 ${
                    activeMenu === item.name ? 'bg-indigo-600 text-white shadow-md' : 'text-gray-700 hover:bg-gray-200'
                  }`}
                >
                  {item.icon}
                  <span>{item.name}</span>
                </button>
              </li>
            ))}
          </ul>
        </nav>
      </div>

      {/* Main Content Area */}
      <div className="flex-1 rounded-xl p-6 shadow-md" style={{ backgroundColor: '#2a4957', color: '#fff' }}>
        <h2 className="text-3xl font-bold mb-6 text-gray-100 flex items-center space-x-3">
          <Briefcase size={30} className="text-indigo-600" />
          <span>Dashibodi ya Afisa Mauzo (SEA)</span>
        </h2>
        {renderContent()}
      </div>
    </div>
  );
};


// Sehemu mpya ya "Maoni ya Wateja" inayotumia Gemini API
const CustomerFeedbackFeature = () => {
  const [feedbackText, setFeedbackText] = useState('');
  const [summary, setSummary] = useState('');
  const [response, setResponse] = useState('');
  const [isLoading, setIsLoading] = useState(false);
  const [error, setError] = useState(null);

  // Kazi ya kuwasiliana na Gemini API
  const callGeminiAPI = async (prompt) => {
    setIsLoading(true);
    setError(null);
    try {
      const chatHistory = [];
      chatHistory.push({ role: "user", parts: [{ text: prompt }] });
      const payload = { contents: chatHistory };
      const apiKey = ""
      const apiUrl = `https://generativelanguage.googleapis.com/v1beta/models/gemini-2.5-flash-preview-05-20:generateContent?key=${apiKey}`;
      
      const apiResponse = await fetch(apiUrl, {
          method: 'POST',
          headers: { 'Content-Type': 'application/json' },
          body: JSON.stringify(payload)
      });

      if (!apiResponse.ok) {
        throw new Error(`API Error: ${apiResponse.statusText}`);
      }

      const result = await apiResponse.json();
      if (result.candidates && result.candidates.length > 0 &&
          result.candidates[0].content && result.candidates[0].content.parts &&
          result.candidates[0].content.parts.length > 0) {
        const text = result.candidates[0].content.parts[0].text;
        return text;
      } else {
        throw new Error('Unexpected API response structure or no content.');
      }
    } catch (err) {
      setError('Kulitokea tatizo. Tafadhali jaribu tena.');
      console.error(err);
      return null;
    } finally {
      setIsLoading(false);
    }
  };

  // Kazi ya kutengeneza muhtasari
  const handleSummarize = async () => {
    if (!feedbackText.trim()) {
      setError('Tafadhali ingiza maoni ya mteja kwanza.');
      return;
    }
    const summaryPrompt = `Fupisha maoni ya mteja yafuatayo kwa sentensi moja tu: ${feedbackText}`;
    const generatedSummary = await callGeminiAPI(summaryPrompt);
    if (generatedSummary) {
      setSummary(generatedSummary);
    }
  };

  // Kazi ya kutengeneza rasimu ya jibu
  const handleDraftResponse = async () => {
    if (!feedbackText.trim()) {
      setError('Tafadhali ingiza maoni ya mteja kwanza.');
      return;
    }
    const responsePrompt = `Andika rasimu ya jibu la heshima na kitaalamu katika lugha ya Kiswahili kwa maoni ya mteja yafuatayo: ${feedbackText}`;
    const generatedResponse = await callGeminiAPI(responsePrompt);
    if (generatedResponse) {
      setResponse(generatedResponse);
    }
  };

  return (
    <div className="p-4">
      <h3 className="text-2xl font-bold mb-4 text-gray-100 flex items-center space-x-2">
        <Star size={24} className="text-indigo-600" />
        <span>Maoni ya Wateja</span>
      </h3>
      <p className="text-gray-200 mb-4">Tumia akili bandia (AI) kuchambua na kujibu maoni ya wateja. Weka maoni hapa chini:</p>
      
      <div className="bg-white rounded-xl p-6 shadow-md mb-6">
        <label htmlFor="feedback-input" className="block text-gray-700 font-medium mb-2">Ingiza Maoni ya Mteja:</label>
        <textarea
          id="feedback-input"
          value={feedbackText}
          onChange={(e) => {
            setFeedbackText(e.target.value);
            setError(null);
          }}
          className="w-full h-32 px-4 py-2 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-indigo-500"
          placeholder="Mfano: 'Huduma yenu ilikuwa nzuri, lakini bidhaa ilikuwa ghali kidogo...'"
        ></textarea>
        {error && <p className="text-red-500 text-sm mt-2">{error}</p>}
        <div className="flex flex-col sm:flex-row space-y-2 sm:space-y-0 sm:space-x-4 mt-4">
          <button
            onClick={handleSummarize}
            disabled={isLoading}
            className="flex-1 flex items-center justify-center space-x-2 px-4 py-2 bg-indigo-600 text-white rounded-lg font-semibold shadow-md hover:bg-indigo-700 transition-all duration-300 disabled:bg-gray-400 disabled:cursor-not-allowed"
          >
            {isLoading ? 'Inatengeneza...' : 'Muhtasari wa Maoni ✨'}
          </button>
          <button
            onClick={handleDraftResponse}
            disabled={isLoading}
            className="flex-1 flex items-center justify-center space-x-2 px-4 py-2 bg-purple-600 text-white rounded-lg font-semibold shadow-md hover:bg-purple-700 transition-all duration-300 disabled:bg-gray-400 disabled:cursor-not-allowed"
          >
            {isLoading ? 'Inatengeneza...' : 'Rasimu ya Jibu ✨'}
          </button>
        </div>
      </div>
      
      {summary && (
        <div className="bg-white rounded-xl p-6 shadow-md mb-6">
          <h4 className="text-xl font-bold text-gray-800 mb-2">Muhtasari wa Maoni:</h4>
          <p className="text-gray-700">{summary}</p>
        </div>
      )}
      
      {response && (
        <div className="bg-white rounded-xl p-6 shadow-md">
          <h4 className="text-xl font-bold text-gray-800 mb-2">Rasimu ya Jibu:</h4>
          <p className="text-gray-700 whitespace-pre-line">{response}</p>
        </div>
      )}
    </div>
  );
};

// Sehemu mpya ya Bidhaa - Inventory Management Component
const ProductsFeature = () => {
    // Bidhaa za mfano zinazohifadhiwa katika state.
    // Katika programu halisi, hizi zitatoka kwenye hifadhidata ya Firestore.
    const [products, setProducts] = useState([
        { id: '1', name: 'Simu janja', sku: 'SJH2024', quantity: 15, purchasePrice: 200000, sellingPrice: 250000 },
        { id: '2', name: 'Kipaza sauti', sku: 'KPS2024', quantity: 50, purchasePrice: 15000, sellingPrice: 25000 },
        { id: '3', name: 'Saa ya mkono', sku: 'SYM2024', quantity: 8, purchasePrice: 45000, sellingPrice: 70000 },
    ]);
    const [formData, setFormData] = useState({
        name: '',
        sku: '',
        quantity: '',
        purchasePrice: '',
        sellingPrice: '',
    });
    const [editingId, setEditingId] = useState(null);
    const [searchQuery, setSearchQuery] = useState('');
    const [statusMessage, setStatusMessage] = useState({ show: false, message: '', type: '' });

    // Kazi ya kushughulikia mabadiliko ya fomu
    const handleChange = (e) => {
        const { name, value } = e.target;
        setFormData({ ...formData, [name]: value });
    };

    // Kazi ya kuwasilisha fomu kuongeza au kuhariri bidhaa
    const handleSubmit = (e) => {
        e.preventDefault();
        const { name, sku, quantity, purchasePrice, sellingPrice } = formData;
        
        // Fomati namba kuwa integers au floats
        const parsedQuantity = parseInt(quantity, 10);
        const parsedPurchasePrice = parseFloat(purchasePrice);
        const parsedSellingPrice = parseFloat(sellingPrice);

        // Uthibitishaji wa fomu
        if (!name || !sku || isNaN(parsedQuantity) || isNaN(parsedPurchasePrice) || isNaN(parsedSellingPrice)) {
            showStatusMessage('Tafadhali jaza taarifa zote kwa usahihi.', 'error');
            return;
        }

        if (editingId) {
            // Hariri bidhaa iliyopo
            setProducts(products.map(p =>
                p.id === editingId ? { ...p, ...formData, quantity: parsedQuantity, purchasePrice: parsedPurchasePrice, sellingPrice: parsedSellingPrice } : p
            ));
            showStatusMessage('Bidhaa imesasishwa kwa mafanikio.', 'success');
        } else {
            // Ongeza bidhaa mpya
            const newProduct = {
                id: Date.now().toString(), // Usimamizi wa kitambulisho cha kipekee
                ...formData,
                quantity: parsedQuantity,
                purchasePrice: parsedPurchasePrice,
                sellingPrice: parsedSellingPrice,
            };
            setProducts([...products, newProduct]);
            showStatusMessage('Bidhaa mpya imeongezwa kwa mafanikio.', 'success');
        }
        
        // Futa fomu na hali ya kuhariri
        setFormData({
            name: '',
            sku: '',
            quantity: '',
            purchasePrice: '',
            sellingPrice: '',
        });
        setEditingId(null);
    };

    // Kazi ya kuweka bidhaa kwenye fomu kwa ajili ya kuhariri
    const handleEditClick = (product) => {
        setEditingId(product.id);
        setFormData({
            name: product.name,
            sku: product.sku,
            quantity: product.quantity,
            purchasePrice: product.purchasePrice,
            sellingPrice: product.sellingPrice,
        });
    };

    // Kazi ya kufuta bidhaa
    const handleDeleteClick = (id) => {
        if (window.confirm('Una uhakika unataka kufuta bidhaa hii?')) {
            setProducts(products.filter(p => p.id !== id));
            showStatusMessage('Bidhaa imefutwa.', 'success');
        }
    };
    
    // Kazi ya kuonyesha ujumbe wa hali
    const showStatusMessage = (message, type) => {
        setStatusMessage({ show: true, message, type });
        setTimeout(() => {
            setStatusMessage({ show: false, message: '', type: '' });
        }, 3000);
    };

    // Kazi ya kuchuja bidhaa kulingana na utafutaji
    const filteredProducts = products.filter(product =>
        product.name.toLowerCase().includes(searchQuery.toLowerCase()) ||
        product.sku.toLowerCase().includes(searchQuery.toLowerCase())
    );

    return (
        <div className="p-4" style={{ backgroundColor: '#2a4957', color: '#fff' }}>
            <h3 className="text-2xl font-bold mb-4 text-gray-100 flex items-center space-x-2">
                <Package size={24} className="text-indigo-600" />
                <span>Usimamizi wa Bidhaa</span>
            </h3>
            <p className="text-gray-200 mb-4">Ongeza, hariri, na simamia bidhaa zako zote hapa. Fuatilia hesabu na faida inayoweza kupatikana.</p>

            {/* Fomu ya Kuongeza/Kuhariri Bidhaa */}
            <div className="bg-white rounded-xl p-6 shadow-md mb-6">
                <h4 className="text-xl font-bold mb-4 flex items-center space-x-2">
                    {editingId ? <Edit size={20} className="text-gray-800" /> : <Plus size={20} className="text-gray-800" />}
                    <span className="text-gray-800">{editingId ? 'Hariri Bidhaa' : 'Ongeza Bidhaa Mpya'}</span>
                </h4>
                <form onSubmit={handleSubmit} className="grid grid-cols-1 md:grid-cols-2 gap-4">
                    <input
                        type="text"
                        name="name"
                        value={formData.name}
                        onChange={handleChange}
                        placeholder="Jina la Bidhaa"
                        className="w-full px-4 py-2 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-indigo-500 text-gray-800"
                    />
                    <input
                        type="text"
                        name="sku"
                        value={formData.sku}
                        onChange={handleChange}
                        placeholder="Namba ya Bidhaa (SKU)"
                        className="w-full px-4 py-2 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-indigo-500 text-gray-800"
                    />
                    <input
                        type="number"
                        name="quantity"
                        value={formData.quantity}
                        onChange={handleChange}
                        placeholder="Idadi (Hisani)"
                        className="w-full px-4 py-2 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-indigo-500 text-gray-800"
                    />
                    <input
                        type="number"
                        name="purchasePrice"
                        value={formData.purchasePrice}
                        onChange={handleChange}
                        placeholder="Bei ya Kununulia"
                        className="w-full px-4 py-2 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-indigo-500 text-gray-800"
                    />
                    <input
                        type="number"
                        name="sellingPrice"
                        value={formData.sellingPrice}
                        onChange={handleChange}
                        placeholder="Bei ya Kuuzia"
                        className="w-full px-4 py-2 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-indigo-500 text-gray-800"
                    />
                    <div className="col-span-1 md:col-span-2 flex justify-end">
                        <button type="submit" className="w-full flex justify-center py-3 px-4 border border-transparent rounded-md shadow-sm text-lg font-medium text-white bg-indigo-600 hover:bg-indigo-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500 transition duration-300 ease-in-out transform hover:-translate-y-1">
                            {editingId ? 'Hifadhi Mabadiliko' : 'Ongeza Bidhaa'}
                        </button>
                    </div>
                </form>
            </div>

            {/* Ujumbe wa Hali */}
            {statusMessage.show && (
                <div className={`p-4 mb-4 rounded-lg text-white font-semibold ${statusMessage.type === 'success' ? 'bg-green-500' : 'bg-red-500'}`}>
                    {statusMessage.message}
                </div>
            )}

            {/* Orodha ya Bidhaa */}
            <div className="bg-white rounded-xl p-6 shadow-md">
                <div className="flex flex-col sm:flex-row justify-between items-center mb-4">
                    <h4 className="text-xl font-bold text-gray-800 mb-2 sm:mb-0">Bidhaa Zilizopo</h4>
                    <div className="relative w-full sm:w-auto">
                        <Search size={20} className="absolute left-3 top-1/2 -translate-y-1/2 text-gray-400" />
                        <input
                            type="text"
                            value={searchQuery}
                            onChange={(e) => setSearchQuery(e.target.value)}
                            placeholder="Tafuta kwa jina au SKU..."
                            className="w-full sm:w-64 px-10 py-2 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-indigo-500 text-gray-800"
                        />
                    </div>
                </div>
                {filteredProducts.length > 0 ? (
                    <div className="overflow-x-auto">
                        <table className="min-w-full bg-white rounded-xl overflow-hidden shadow-sm">
                            <thead>
                                <tr className="bg-gray-200 text-left text-gray-600 font-semibold uppercase text-sm">
                                    <th className="py-3 px-6">Jina la Bidhaa</th>
                                    <th className="py-3 px-6">SKU</th>
                                    <th className="py-3 px-6 text-center">Hisani</th>
                                    <th className="py-3 px-6">Bei ya Kununulia</th>
                                    <th className="py-3 px-6">Bei ya Kuuzia</th>
                                    <th className="py-3 px-6">Faida/Bidhaa</th>
                                    <th className="py-3 px-6 text-center">Vitendo</th>
                                </tr>
                            </thead>
                            <tbody>
                                {filteredProducts.map(product => {
                                    const profitPerItem = product.sellingPrice - product.purchasePrice;
                                    const profitMargin = (profitPerItem / product.sellingPrice) * 100;

                                    return (
                                        <tr key={product.id} className="border-t border-gray-200 hover:bg-gray-50 transition-colors duration-200">
                                            <td className="py-4 px-6 font-medium text-gray-700">{product.name}</td>
                                            <td className="py-4 px-6 text-gray-600">{product.sku}</td>
                                            <td className="py-4 px-6 text-center text-gray-600 font-bold">{product.quantity}</td>
                                            <td className="py-4 px-6 text-gray-600">Tsh {product.purchasePrice.toLocaleString()}</td>
                                            <td className="py-4 px-6 text-gray-600">Tsh {product.sellingPrice.toLocaleString()}</td>
                                            <td className="py-4 px-6 text-gray-600">
                                                Tsh {profitPerItem.toLocaleString()} <span className="text-xs">({profitMargin.toFixed(1)}%)</span>
                                            </td>
                                            <td className="py-4 px-6 text-center space-x-2">
                                                <button
                                                    onClick={() => handleEditClick(product)}
                                                    className="p-2 text-indigo-600 rounded-lg hover:bg-indigo-100 transition-colors"
                                                    title="Hariri"
                                                >
                                                    <Edit size={18} />
                                                </button>
                                                <button
                                                    onClick={() => handleDeleteClick(product.id)}
                                                    className="p-2 text-red-600 rounded-lg hover:bg-red-100 transition-colors"
                                                    title="Futa"
                                                >
                                                    <Trash2 size={18} />
                                                </button>
                                            </td>
                                        </tr>
                                    );
                                })}
                            </tbody>
                        </table>
                    </div>
                ) : (
                    <p className="text-gray-500 italic text-center">Hakuna bidhaa zilizopatikana.</p>
                )}
            </div>
        </div>
    );
};


// Reusable placeholder component for content
const PlaceholderContent = ({ title, description }) => (
  <div className="p-4 text-center bg-white rounded-lg shadow-inner">
    <h3 className="text-2xl font-bold text-gray-700">{title}</h3>
    <p className="text-gray-500 mt-2">{description}</p>
    <p className="text-sm text-gray-400 mt-4 italic">Kipengele hiki bado kinajengwa.</p>
  </div>
);

// New Registration component
const Register = () => {
    const { navigateTo } = useContext(AuthContext);

    const [activeForm, setActiveForm] = useState('single-user');
    const [popupContent, setPopupContent] = useState(null);
  
    const showPopup = (content) => {
      setPopupContent(content);
    };
  
    const closePopup = () => {
      setPopupContent(null);
    };
  
    const singleUserDescription = (
      <div className="space-y-2 text-gray-700">
        <h3 className="text-xl font-bold mb-4 text-gray-800">Akaunti ya Msimamizi Mmoja (Single User Management)</h3>
        <p>Hii ni aina ya akaunti inayosimamiwa na mtu mmoja pekee.</p>
        <p>Mmiliki wa akaunti ndiye anayepata ufikiaji wa vipengele vyote vya mfumo ikiwa ni pamoja na bidhaa, mauzo, manunuzi, matumizi, hesabu na taarifa zote.</p>
        <p>Inafaa kwa wajasiriamali binafsi au biashara ndogo zinazohitaji mtu mmoja kusimamia kila kitu moja kwa moja.</p>
      </div>
    );
  
    const multiUserDescription = (
      <div className="space-y-2 text-gray-700">
        <h3 className="text-xl font-bold mb-4 text-gray-800">Akaunti ya Usimamizi kwa Mgao wa Majukumu (Role-Based Management)</h3>
        <p>Aina hii ya akaunti inamruhusu System Admin kufungua akaunti kuu na kisha kusajili watumiaji wengine chini yake.</p>
        <p>System Admin anaweza kugawa majukumu kwa kila mtumiaji au idara (mfano: Mauzo, Manunuzi, Matumizi, Ripoti).</p>
        <p>Kila mtumiaji ataona na kufanyia kazi tu jukumu au idara aliyopangiwa.</p>
        <p>Inafaa kwa kampuni, taasisi au biashara zinazohitaji usimamizi wa pamoja na mgawanyo wa majukumu.</p>
      </div>
    );

    const handleSubmit = (e) => {
      e.preventDefault();
      // Using a custom message box instead of `alert()`
      showPopup(
        <div className="text-center">
            <h3 className="text-xl font-bold text-gray-800 mb-4">Ujumbe</h3>
            <p className="text-gray-700 mb-4">Fomu ya {activeForm === 'single-user' ? 'Akaunti ya Msingi' : 'Akaunti ya Timu'} imetumwa (simulizi).</p>
            <button onClick={closePopup} className="px-4 py-2 bg-indigo-600 text-white rounded-lg hover:bg-indigo-700">Sawa</button>
        </div>
    );
    };
  
    return (
      <div className="p-4" style={{ backgroundColor: '#2a4957', color: '#fff' }}>
        <header className="mb-8 text-center">
            <h1 className="text-xl md:text-2xl font-bold mb-2">Aina za Usajili</h1>
            <p className="text-gray-200">Chagua aina ya akaunti ya usimamizi unayotaka kuisajili.</p>
        </header>

        {/* Selection Buttons and Info Icons */}
        <div className="flex flex-col md:flex-row justify-center gap-4 mb-8">
            <div className="relative w-full md:w-1/2">
                <button
                    onClick={() => setActiveForm('single-user')}
                    className={`w-full py-4 px-6 text-lg font-semibold rounded-lg shadow-md transition-all duration-300 transform hover:scale-105 border-2 border-gray-200 ${activeForm === 'single-user' ? 'bg-gray-800 text-white' : 'bg-white text-gray-800 hover:bg-gray-200'}`}
                >
                    Msimamizi Mmoja
                </button>
                <button
                    onClick={() => showPopup(singleUserDescription)}
                    className="absolute top-3 right-3 text-gray-500 hover:text-gray-700 transition-colors duration-200"
                    title="Taarifa zaidi"
                >
                    <Info size={24} />
                </button>
            </div>

            <div className="relative w-full md:w-1/2">
                <button
                    onClick={() => setActiveForm('multi-user')}
                    className={`w-full py-4 px-6 text-lg font-semibold rounded-lg shadow-md transition-all duration-300 transform hover:scale-105 border-2 border-gray-200 ${activeForm === 'multi-user' ? 'bg-gray-800 text-white' : 'bg-white text-gray-800 hover:bg-bg-gray-200'}`}
                >
                    Msimamizi Zaidi ya Mmoja
                </button>
                <button
                    onClick={() => showPopup(multiUserDescription)}
                    className="absolute top-3 right-3 text-gray-500 hover:text-gray-700 transition-colors duration-200"
                    title="Taarifa zaidi"
                >
                    <Info size={24} />
                </button>
            </div>
        </div>
        
        {/* Registration Forms */}
        <div className="mt-8">
            {activeForm === 'single-user' && (
                <div id="form-single-user" className="form-container active">
                    <form onSubmit={handleSubmit} className="space-y-6 bg-white p-6 rounded-lg shadow-inner">
                        <h2 className="text-2xl font-semibold text-gray-800 text-center">Fomu ya Akaunti ya Msingi</h2>
                        <div>
                            <label htmlFor="name-single" className="block text-sm font-medium text-gray-700">Jina Kamili</label>
                            <input type="text" id="name-single" name="name-single" placeholder="Mfano: Juma Ally" className="mt-1 block w-full px-4 py-2 border border-gray-300 rounded-md shadow-sm focus:ring-indigo-500 focus:border-indigo-500 sm:text-sm"/>
                        </div>
                        <div>
                            <label htmlFor="email-single" className="block text-sm font-medium text-gray-700">Barua pepe</label>
                            <input type="email" id="email-single" name="email-single" placeholder="barua_pepe@example.com" className="mt-1 block w-full px-4 py-2 border border-gray-300 rounded-md shadow-sm focus:ring-indigo-500 focus:border-indigo-500 sm:text-sm"/>
                        </div>
                        <div>
                            <label htmlFor="password-single" className="block text-sm font-medium text-gray-700">Nenosiri</label>
                            <input type="password" id="password-single" name="password-single" placeholder="Weka nenosiri salama" className="mt-1 block w-full px-4 py-2 border border-gray-300 rounded-md shadow-sm focus:ring-indigo-500 focus:border-indigo-500 sm:text-sm"/>
                        </div>
                        <div>
                            <button type="submit" className="w-full flex justify-center py-3 px-4 border border-transparent rounded-md shadow-sm text-lg font-medium text-white bg-indigo-600 hover:bg-indigo-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500 transition duration-300 ease-in-out transform hover:-translate-y-1">
                                Jisajili
                            </button>
                        </div>
                    </form>
                </div>
            )}
            
            {activeForm === 'multi-user' && (
                <div id="form-multi-user" className="form-container active">
                    <form onSubmit={handleSubmit} className="space-y-6 bg-white p-6 rounded-lg shadow-inner">
                        <h2 className="text-2xl font-semibold text-gray-800 text-center">Fomu ya Akaunti ya Timu</h2>
                        <div>
                            <label htmlFor="company-name" className="block text-sm font-medium text-gray-700">Jina la Kampuni/Taasisi</label>
                            <input type="text" id="company-name" name="company-name" placeholder="Mfano: Fasta Solutions" className="mt-1 block w-full px-4 py-2 border border-gray-300 rounded-md shadow-sm focus:ring-indigo-500 focus:border-indigo-500 sm:text-sm"/>
                        </div>
                        <div>
                            <label htmlFor="admin-name" className="block text-sm font-medium text-gray-700">Jina la Msimamizi (Admin)</label>
                            <input type="text" id="admin-name" name="admin-name" placeholder="Jina la mtumiaji mkuu" className="mt-1 block w-full px-4 py-2 border border-gray-300 rounded-md shadow-sm focus:ring-indigo-500 focus:border-border-indigo-500 sm:text-sm"/>
                        </div>
                        <div>
                            <label htmlFor="admin-email" className="block text-sm font-medium text-gray-700">Barua pepe ya Msimamizi</label>
                            <input type="email" id="admin-email" name="admin-email" placeholder="barua_pepe@company.com" className="mt-1 block w-full px-4 py-2 border border-gray-300 rounded-md shadow-sm focus:ring-indigo-500 focus:border-indigo-500 sm:text-sm"/>
                        </div>
                        <div>
                            <label htmlFor="admin-password" className="block text-sm font-medium text-gray-700">Nenosiri la Msimamizi</label>
                            <input type="password" id="admin-password" name="admin-password" placeholder="Nenosiri salama la msimamizi" className="mt-1 block w-full px-4 py-2 border border-gray-300 rounded-md shadow-sm focus:ring-indigo-500 focus:border-indigo-500 sm:text-sm"/>
                        </div>
                        <div>
                            <button type="submit" className="w-full flex justify-center py-3 px-4 border border-transparent rounded-md shadow-sm text-lg font-medium text-white bg-indigo-600 hover:bg-indigo-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500 transition duration-300 ease-in-out transform hover:-translate-y-1">
                                Jisajili
                            </button>
                        </div>
                    </form>
                </div>
            )}
        </div>

        {/* Modal/Popup component */}
        {popupContent && (
            <div className="fixed inset-0 z-50 flex items-center justify-center p-4 bg-gray-900 bg-opacity-70 backdrop-filter backdrop-blur-sm">
                <div className="bg-white rounded-lg p-6 shadow-xl max-w-lg w-full relative">
                    <button
                        onClick={closePopup}
                        className="absolute top-3 right-3 text-gray-500 hover:text-gray-900 transition-colors"
                    >
                        <X size={24} />
                    </button>
                    {popupContent}
                </div>
            </div>
        )}
      </div>
    );
};

export default App;
