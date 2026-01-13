import React, { useState } from 'react';
import { ShoppingCart, Heart, User, Menu, X, Star, Package, MessageSquare, Phone, AlertCircle, Home, Box, Mail, Info } from 'lucide-react';

const EuphoriaEcommerce = () => {
  const [currentPage, setCurrentPage] = useState('home');
  const [isMenuOpen, setIsMenuOpen] = useState(false);
  const [isLoggedIn, setIsLoggedIn] = useState(false);
  const [showAuthModal, setShowAuthModal] = useState(false);
  const [authMode, setAuthMode] = useState('login');
  const [cart, setCart] = useState([]);
  const [wishlist, setWishlist] = useState([]);
  
  const [loginForm, setLoginForm] = useState({ email: '', password: '' });
  const [registerForm, setRegisterForm] = useState({
    name: '', email: '', password: '', confirmPassword: '', phone: ''
  });
  const [formErrors, setFormErrors] = useState({});

  const products = [
    { id: 1, name: 'Wireless Headphones', price: 79.99, image: '🎧', category: 'Electronics', rating: 4.5, reviews: 234 },
    { id: 2, name: 'Smart Watch', price: 199.99, image: '⌚', category: 'Electronics', rating: 4.8, reviews: 567 },
    { id: 3, name: 'Designer Backpack', price: 49.99, image: '🎒', category: 'Fashion', rating: 4.3, reviews: 123 },
    { id: 4, name: 'Running Shoes', price: 89.99, image: '👟', category: 'Sports', rating: 4.6, reviews: 890 },
    { id: 5, name: 'Coffee Maker', price: 129.99, image: '☕', category: 'Home', rating: 4.7, reviews: 456 },
    { id: 6, name: 'Yoga Mat', price: 29.99, image: '🧘', category: 'Sports', rating: 4.4, reviews: 678 },
  ];

  const orders = [
    { id: 'ORD001', date: '2025-11-10', status: 'Delivered', total: 279.97, items: 3 },
    { id: 'ORD002', date: '2025-11-12', status: 'Shipped', total: 159.98, items: 2 },
    { id: 'ORD003', date: '2025-11-13', status: 'Processing', total: 89.99, items: 1 },
  ];

  const reviews = [
    { id: 1, product: 'Wireless Headphones', rating: 5, comment: 'Amazing sound quality! Highly recommended.', user: 'John D.', date: '2025-11-08' },
    { id: 2, product: 'Smart Watch', rating: 4, comment: 'Great features but battery could be better.', user: 'Sarah M.', date: '2025-11-09' },
  ];

  const validateEmail = (email) => /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(email);
  const validatePhone = (phone) => /^[0-9]{10}$/.test(phone);
  const validatePassword = (password) => password.length >= 8;

  const validateLoginForm = () => {
    const errors = {};
    if (!loginForm.email) errors.email = 'Email is required';
    else if (!validateEmail(loginForm.email)) errors.email = 'Invalid email format';
    if (!loginForm.password) errors.password = 'Password is required';
    setFormErrors(errors);
    return Object.keys(errors).length === 0;
  };

  const validateRegisterForm = () => {
    const errors = {};
    if (!registerForm.name) errors.name = 'Name is required';
    else if (registerForm.name.length < 3) errors.name = 'Name must be at least 3 characters';
    if (!registerForm.email) errors.email = 'Email is required';
    else if (!validateEmail(registerForm.email)) errors.email = 'Invalid email format';
    if (!registerForm.phone) errors.phone = 'Phone is required';
    else if (!validatePhone(registerForm.phone)) errors.phone = 'Phone must be 10 digits';
    if (!registerForm.password) errors.password = 'Password is required';
    else if (!validatePassword(registerForm.password)) errors.password = 'Password must be at least 8 characters';
    if (registerForm.password !== registerForm.confirmPassword) errors.confirmPassword = 'Passwords do not match';
    setFormErrors(errors);
    return Object.keys(errors).length === 0;
  };

  const handleLogin = (e) => {
    e.preventDefault();
    if (validateLoginForm()) {
      setIsLoggedIn(true);
      setShowAuthModal(false);
      setLoginForm({ email: '', password: '' });
      setFormErrors({});
    }
  };

  const handleRegister = (e) => {
    e.preventDefault();
    if (validateRegisterForm()) {
      setIsLoggedIn(true);
      setShowAuthModal(false);
      setRegisterForm({ name: '', email: '', password: '', confirmPassword: '', phone: '' });
      setFormErrors({});
    }
  };

  const Navigation = () => (
    <nav className="bg-gradient-to-r from-purple-600 to-pink-600 text-white shadow-lg sticky top-0 z-50">
      <div className="max-w-7xl mx-auto px-4">
        <div className="flex justify-between items-center h-16">
          <div className="flex items-center space-x-8">
            <h1 className="text-2xl font-bold cursor-pointer" onClick={() => setCurrentPage('home')}>✨ Euphoria</h1>
            <div className="hidden md:flex space-x-6">
              <button onClick={() => setCurrentPage('home')} className="flex items-center hover:text-pink-200 transition">
                <Home className="w-4 h-4 mr-1" /> Home
              </button>
              <button onClick={() => setCurrentPage('products')} className="flex items-center hover:text-pink-200 transition">
                <Box className="w-4 h-4 mr-1" /> Products
              </button>
              <button onClick={() => setCurrentPage('contact')} className="flex items-center hover:text-pink-200 transition">
                <Mail className="w-4 h-4 mr-1" /> Contact Us
              </button>
              <button onClick={() => setCurrentPage('about')} className="flex items-center hover:text-pink-200 transition">
                <Info className="w-4 h-4 mr-1" /> About Us
              </button>
            </div>
          </div>
          
          <div className="flex items-center space-x-4">
            <button onClick={() => setCurrentPage('wishlist')} className="relative hover:text-pink-200 transition">
              <Heart className="w-6 h-6" />
              {wishlist.length > 0 && (
                <span className="absolute -top-2 -right-2 bg-red-500 text-white text-xs rounded-full w-5 h-5 flex items-center justify-center">{wishlist.length}</span>
              )}
            </button>
            <button onClick={() => setCurrentPage('cart')} className="relative hover:text-pink-200 transition">
              <ShoppingCart className="w-6 h-6" />
              {cart.length > 0 && (
                <span className="absolute -top-2 -right-2 bg-red-500 text-white text-xs rounded-full w-5 h-5 flex items-center justify-center">{cart.length}</span>
              )}
            </button>
            {isLoggedIn ? (
              <button onClick={() => setCurrentPage('account')} className="hover:text-pink-200 transition">
                <User className="w-6 h-6" />
              </button>
            ) : (
              <button onClick={() => { setShowAuthModal(true); setAuthMode('login'); }} className="bg-white text-purple-600 px-4 py-2 rounded-full font-semibold hover:bg-pink-100 transition">Login</button>
            )}
            <button onClick={() => setIsMenuOpen(!isMenuOpen)} className="md:hidden">
              {isMenuOpen ? <X className="w-6 h-6" /> : <Menu className="w-6 h-6" />}
            </button>
          </div>
        </div>
        
        {isMenuOpen && (
          <div className="md:hidden pb-4 space-y-2">
            <button onClick={() => { setCurrentPage('home'); setIsMenuOpen(false); }} className="block w-full text-left py-2 hover:text-pink-200">Home</button>
            <button onClick={() => { setCurrentPage('products'); setIsMenuOpen(false); }} className="block w-full text-left py-2 hover:text-pink-200">Products</button>
            <button onClick={() => { setCurrentPage('contact'); setIsMenuOpen(false); }} className="block w-full text-left py-2 hover:text-pink-200">Contact Us</button>
            <button onClick={() => { setCurrentPage('about'); setIsMenuOpen(false); }} className="block w-full text-left py-2 hover:text-pink-200">About Us</button>
          </div>
        )}
      </div>
    </nav>
  );

  const AuthModal = () => (
    <div className="fixed inset-0 bg-black bg-opacity-50 flex items-center justify-center z-50 p-4">
      <div className="bg-white rounded-2xl max-w-md w-full p-8 relative max-h-[90vh] overflow-y-auto">
        <button onClick={() => { setShowAuthModal(false); setFormErrors({}); }} className="absolute top-4 right-4 text-gray-500 hover:text-gray-700">
          <X className="w-6 h-6" />
        </button>
        
        <h2 className="text-3xl font-bold text-purple-600 mb-6">{authMode === 'login' ? 'Login' : 'Register'}</h2>
        
        {authMode === 'login' ? (
          <form onSubmit={handleLogin} className="space-y-4">
            <div>
              <label className="block text-gray-700 font-semibold mb-2">Email</label>
              <input type="email" value={loginForm.email} onChange={(e) => setLoginForm({ ...loginForm, email: e.target.value })}
                className="w-full px-4 py-2 border-2 border-gray-300 rounded-lg focus:border-purple-500 focus:outline-none" placeholder="your@email.com" />
              {formErrors.email && <p className="text-red-500 text-sm mt-1">{formErrors.email}</p>}
            </div>
            <div>
              <label className="block text-gray-700 font-semibold mb-2">Password</label>
              <input type="password" value={loginForm.password} onChange={(e) => setLoginForm({ ...loginForm, password: e.target.value })}
                className="w-full px-4 py-2 border-2 border-gray-300 rounded-lg focus:border-purple-500 focus:outline-none" placeholder="••••••••" />
              {formErrors.password && <p className="text-red-500 text-sm mt-1">{formErrors.password}</p>}
            </div>
            <button type="submit" className="w-full bg-gradient-to-r from-purple-600 to-pink-600 text-white py-3 rounded-lg font-semibold hover:shadow-lg transition">Login</button>
          </form>
        ) : (
          <form onSubmit={handleRegister} className="space-y-4">
            <div>
              <label className="block text-gray-700 font-semibold mb-2">Full Name</label>
              <input type="text" value={registerForm.name} onChange={(e) => setRegisterForm({ ...registerForm, name: e.target.value })}
                className="w-full px-4 py-2 border-2 border-gray-300 rounded-lg focus:border-purple-500 focus:outline-none" placeholder="John Doe" />
              {formErrors.name && <p className="text-red-500 text-sm mt-1">{formErrors.name}</p>}
            </div>
            <div>
              <label className="block text-gray-700 font-semibold mb-2">Email</label>
              <input type="email" value={registerForm.email} onChange={(e) => setRegisterForm({ ...registerForm, email: e.target.value })}
                className="w-full px-4 py-2 border-2 border-gray-300 rounded-lg focus:border-purple-500 focus:outline-none" placeholder="your@email.com" />
              {formErrors.email && <p className="text-red-500 text-sm mt-1">{formErrors.email}</p>}
            </div>
            <div>
              <label className="block text-gray-700 font-semibold mb-2">Phone</label>
              <input type="tel" value={registerForm.phone} onChange={(e) => setRegisterForm({ ...registerForm, phone: e.target.value })}
                className="w-full px-4 py-2 border-2 border-gray-300 rounded-lg focus:border-purple-500 focus:outline-none" placeholder="1234567890" />
              {formErrors.phone && <p className="text-red-500 text-sm mt-1">{formErrors.phone}</p>}
            </div>
            <div>
              <label className="block text-gray-700 font-semibold mb-2">Password</label>
              <input type="password" value={registerForm.password} onChange={(e) => setRegisterForm({ ...registerForm, password: e.target.value })}
                className="w-full px-4 py-2 border-2 border-gray-300 rounded-lg focus:border-purple-500 focus:outline-none" placeholder="Min 8 characters" />
              {formErrors.password && <p className="text-red-500 text-sm mt-1">{formErrors.password}</p>}
            </div>
            <div>
              <label className="block text-gray-700 font-semibold mb-2">Confirm Password</label>
              <input type="password" value={registerForm.confirmPassword} onChange={(e) => setRegisterForm({ ...registerForm, confirmPassword: e.target.value })}
                className="w-full px-4 py-2 border-2 border-gray-300 rounded-lg focus:border-purple-500 focus:outline-none" placeholder="••••••••" />
              {formErrors.confirmPassword && <p className="text-red-500 text-sm mt-1">{formErrors.confirmPassword}</p>}
            </div>
            <button type="submit" className="w-full bg-gradient-to-r from-purple-600 to-pink-600 text-white py-3 rounded-lg font-semibold hover:shadow-lg transition">Register</button>
          </form>
        )}
        
        <div className="mt-4 text-center">
          <button onClick={() => { setAuthMode(authMode === 'login' ? 'register' : 'login'); setFormErrors({}); }} className="text-purple-600 hover:underline">
            {authMode === 'login' ? "Don't have an account? Register" : 'Already have an account? Login'}
          </button>
        </div>
      </div>
    </div>
  );

  return (
    <div className="min-h-screen bg-gray-50">
      <Navigation />
      {showAuthModal && <AuthModal />}
      
      {currentPage === 'home' && (
        <div>
          <div className="bg-gradient-to-r from-purple-500 to-pink-500 text-white py-20 px-4">
            <div className="max-w-4xl mx-auto text-center">
              <h1 className="text-5xl font-bold mb-4">Welcome to Euphoria</h1>
              <p className="text-xl mb-8">Discover joy in every purchase</p>
              <button onClick={() => setCurrentPage('products')} className="bg-white text-purple-600 px-8 py-3 rounded-full font-bold text-lg hover:shadow-2xl transition">Shop Now</button>
            </div>
          </div>
          <div className="max-w-7xl mx-auto px-4 py-16">
            <h2 className="text-3xl font-bold text-center mb-12 text-gray-800">Featured Products</h2>
            <div className="grid grid-cols-1 md:grid-cols-3 gap-8">
              {products.slice(0, 3).map(product => (
                <div key={product.id} className="bg-white rounded-xl shadow-lg overflow-hidden hover:shadow-2xl transition transform hover:-translate-y-2">
                  <div className="text-6xl text-center py-8 bg-gradient-to-br from-purple-100 to-pink-100">{product.image}</div>
                  <div className="p-6">
                    <h3 className="font-bold text-xl mb-2">{product.name}</h3>
                    <div className="flex items-center mb-2">
                      <Star className="w-4 h-4 text-yellow-400 fill-current" />
                      <span className="ml-1 text-sm text-gray-600">{product.rating} ({product.reviews})</span>
                    </div>
                    <p className="text-2xl font-bold text-purple-600 mb-4">${product.price}</p>
                    <button onClick={() => setCart([...cart, product])} className="w-full bg-gradient-to-r from-purple-600 to-pink-600 text-white py-2 rounded-lg font-semibold hover:shadow-lg transition">Add to Cart</button>
                  </div>
                </div>
              ))}
            </div>
          </div>
        </div>
      )}

      {currentPage === 'products' && (
        <div className="max-w-7xl mx-auto px-4 py-8">
          <h2 className="text-3xl font-bold mb-8 text-gray-800">All Products</h2>
          <div className="grid grid-cols-1 md:grid-cols-3 lg:grid-cols-4 gap-6">
            {products.map(product => (
              <div key={product.id} className="bg-white rounded-xl shadow-lg overflow-hidden hover:shadow-2xl transition">
                <div className="text-6xl text-center py-8 bg-gradient-to-br from-purple-100 to-pink-100">{product.image}</div>
                <div className="p-4">
                  <h3 className="font-bold text-lg mb-1">{product.name}</h3>
                  <p className="text-sm text-gray-500 mb-2">{product.category}</p>
                  <div className="flex items-center mb-2">
                    <Star className="w-4 h-4 text-yellow-400 fill-current" />
                    <span className="ml-1 text-sm text-gray-600">{product.rating}</span>
                  </div>
                  <p className="text-xl font-bold text-purple-600 mb-3">${product.price}</p>
                  <div className="flex space-x-2">
                    <button onClick={() => setCart([...cart, product])} className="flex-1 bg-purple-600 text-white py-2 rounded-lg text-sm font-semibold hover:bg-purple-700 transition">Add to Cart</button>
                    <button onClick={() => !wishlist.find(i => i.id === product.id) && setWishlist([...wishlist, product])} className="bg-pink-100 text-pink-600 p-2 rounded-lg hover:bg-pink-200 transition">
                      <Heart className="w-5 h-5" />
                    </button>
                  </div>
                </div>
              </div>
            ))}
          </div>
        </div>
      )}

      {currentPage === 'account' && (
        <div className="max-w-7xl mx-auto px-4 py-8">
          <h2 className="text-3xl font-bold mb-8 text-gray-800">My Account</h2>
          
          <div className="bg-white rounded-xl shadow-lg p-6 mb-8">
            <h3 className="text-2xl font-bold mb-4 flex items-center text-purple-600">
              <Package className="w-6 h-6 mr-2" /> My Orders
            </h3>
            <div className="space-y-4">
              {orders.map(order => (
                <div key={order.id} className="border-2 border-gray-200 rounded-lg p-4 hover:border-purple-400 transition">
                  <div className="flex justify-between items-center mb-2">
                    <span className="font-bold text-lg">Order #{order.id}</span>
                    <span className={`px-3 py-1 rounded-full text-sm font-semibold ${
                      order.status === 'Delivered' ? 'bg-green-100 text-green-600' :
                      order.status === 'Shipped' ? 'bg-blue-100 text-blue-600' : 'bg-yellow-100 text-yellow-600'}`}>
                      {order.status}
                    </span>
                  </div>
                  <p className="text-gray-600">Date: {order.date} | Items: {order.items}</p>
                  <p className="text-xl font-bold text-purple-600 mt-2">Total: ${order.total}</p>
                </div>
              ))}
            </div>
          </div>

          <div className="bg-white rounded-xl shadow-lg p-6 mb-8">
            <h3 className="text-2xl font-bold mb-4 flex items-center text-purple-600">
              <MessageSquare className="w-6 h-6 mr-2" /> My Reviews
            </h3>
            <div className="space-y-4">
              {reviews.map(review => (
                <div key={review.id} className="border-2 border-gray-200 rounded-lg p-4">
                  <div className="flex items-center justify-between mb-2">
                    <span className="font-bold">{review.product}</span>
                    <div className="flex">
                      {[...Array(5)].map((_, i) => (
                        <Star key={i} className={`w-4 h-4 ${i < review.rating ? 'text-yellow-400 fill-current' : 'text-gray-300'}`} />
                      ))}
                    </div>
                  </div>
                  <p className="text-gray-700 mb-2">{review.comment}</p>
                  <p className="text-sm text-gray-500">{review.date}</p>
                </div>
              ))}
            </div>
          </div>

          <div className="bg-white rounded-xl shadow-lg p-6">
            <h3 className="text-2xl font-bold mb-4 flex items-center text-purple-600">
              <Phone className="w-6 h-6 mr-2" /> Customer Care & Reporting
            </h3>
            <div className="grid md:grid-cols-2 gap-4">
              <div className="border-2 border-gray-200 rounded-lg p-4 hover:border-purple-400 transition cursor-pointer">
                <Phone className="w-8 h-8 text-purple-600 mb-2" />
                <h4 className="font-bold mb-2">Contact Support</h4>
