# TR
import { useState } from 'react'
import { Button } from '@/components/ui/button.jsx'
import { Card, CardContent, CardDescription, CardHeader, CardTitle } from '@/components/ui/card.jsx'
import { Badge } from '@/components/ui/badge.jsx'
import { Download, Smartphone, Monitor, Tablet, Menu, X, Heart, ThumbsUp, Star, Home } from 'lucide-react'
import { motion, AnimatePresence } from 'framer-motion'
import './App.css'

function App() {
  const [selectedCategory, setSelectedCategory] = useState('home')
  const [mobileMenuOpen, setMobileMenuOpen] = useState(false)
  const [reactions, setReactions] = useState({})
  const [userReactions, setUserReactions] = useState({})
  const [downloadClicks, setDownloadClicks] = useState({})

  const hacks = {
    iphone: [
      {
        id: 1,
        name: "هاك دلتا ايفون",
        description: "أداة قوية لتشغيل الألعاب والتطبيقات المعدلة على iOS",
        downloadUrl: "itms-services://?action=download-manifest&url=https%3A%2F%2Fdeltaios-executor.com%2FInstall1.plist",
        category: "iOS Executor",
        status: "متاح"
      },
      {
        id: 2,
        name: "هام كرنل ايفون",
        description: "محرك تنفيذ متقدم للسكريبتات والأدوات على iOS",
        downloadUrl: "itms-services://?action=download-manifest&url=https%3A%2F%2Fkrnl-ios.com%2FInstall1.plist",
        category: "Script Executor",
        status: "متاح"
      },
      {
        id: 3,
        name: "هاك كوديكس ايفون",
        description: "منصة شاملة لتشغيل الأدوات والتطبيقات المعدلة",
        downloadUrl: "itms-services://?action=download-manifest&url=https%3A%2F%2Fcodex-ios.com%2FInstall.plist",
        category: "Multi-Tool",
        status: "متاح"
      }
    ],
    android: [
      {
        id: 4,
        name: "هاك دلتا اندرويد",
        description: "نسخة أندرويد من أداة دلتا القوية للألعاب والتطبيقات",
        downloadUrl: "https://www.mediafire.com/file/0vpx0t6ilzryph6/Delta-2.679.762.apk/file",
        category: "Game Executor",
        status: "متاح"
      }
    ],
    pc: []
  }

  const categories = [
    { id: 'home', name: 'الصفحة الرئيسية', icon: Home },
    { id: 'iphone', name: 'هاكات iPhone', icon: Smartphone },
    { id: 'android', name: 'هاكات Android', icon: Tablet },
    { id: 'pc', name: 'هاكات PC', icon: Monitor }
  ]

  const reactionTypes = [
    { id: 'like', icon: ThumbsUp, color: 'text-blue-400', bgColor: 'bg-blue-500/20' },
    { id: 'love', icon: Heart, color: 'text-red-400', bgColor: 'bg-red-500/20' },
    { id: 'star', icon: Star, color: 'text-yellow-400', bgColor: 'bg-yellow-500/20' }
  ]

  const getFilteredHacks = () => {
    if (selectedCategory === 'home') {
      return [] // No hacks to display on home page, only instructions
    }
    return hacks[selectedCategory] || []
  }

  const handleDownload = (url, name, hackId) => {
    // إضافة رد فعل عند الضغط على زر التحميل
    setDownloadClicks(prev => ({
      ...prev,
      [hackId]: (prev[hackId] || 0) + 1
    }))
    
    // فتح رابط التحميل
    window.open(url, '_blank')
  }

  const handleReaction = (hackId, reactionType) => {
    setReactions(prev => ({
      ...prev,
      [hackId]: {
        ...prev[hackId],
        [reactionType]: (prev[hackId]?.[reactionType] || 0) + 1
      }
    }))

    setUserReactions(prev => ({
      ...prev,
      [hackId]: reactionType
    }))
  }

  const getReactionCount = (hackId, reactionType) => {
    return reactions[hackId]?.[reactionType] || Math.floor(Math.random() * 50) + 10
  }

  const getDownloadCount = (hackId) => {
    return downloadClicks[hackId] || Math.floor(Math.random() * 100) + 50
  }

  return (
    <div className="min-h-screen bg-gradient-to-br from-slate-900 via-purple-900 to-slate-900">
      {/* Header */}
      <header className="bg-black/20 backdrop-blur-md border-b border-white/10 sticky top-0 z-50">
        <div className="container mx-auto px-4 py-4">
          <div className="flex items-center justify-between">
            <motion.div 
              initial={{ opacity: 0, x: -20 }}
              animate={{ opacity: 1, x: 0 }}
              className="flex items-center space-x-3"
            >
              <div className="w-10 h-10 bg-gradient-to-r from-purple-500 to-pink-500 rounded-lg flex items-center justify-center">
                <Download className="w-6 h-6 text-white" />
              </div>
              <div>
                <h1 className="text-2xl font-bold text-white">ZRF</h1>
                <p className="text-purple-300 text-sm">مركز الهاكات والأدوات</p>
              </div>
            </motion.div>

            {/* Desktop Navigation */}
            <nav className="hidden md:flex space-x-6">
              {categories.map((category) => {
                const Icon = category.icon
                return (
                  <Button
                    key={category.id}
                    variant={selectedCategory === category.id ? "default" : "ghost"}
                    onClick={() => setSelectedCategory(category.id)}
                    className="text-white hover:text-purple-300 transition-colors"
                  >
                    <Icon className="w-4 h-4 mr-2" />
                    {category.name}
                  </Button>
                )
              })}
            </nav>

            {/* Mobile Menu Button */}
            <Button
              variant="ghost"
              size="icon"
              className="md:hidden text-white"
              onClick={() => setMobileMenuOpen(!mobileMenuOpen)}
            >
              {mobileMenuOpen ? <X className="w-6 h-6" /> : <Menu className="w-6 h-6" />}
            </Button>
          </div>

          {/* Mobile Navigation */}
          <AnimatePresence>
            {mobileMenuOpen && (
              <motion.nav
                initial={{ opacity: 0, height: 0 }}
                animate={{ opacity: 1, height: 'auto' }}
                exit={{ opacity: 0, height: 0 }}
                className="md:hidden mt-4 space-y-2"
              >
                {categories.map((category) => {
                  const Icon = category.icon
                  return (
                    <Button
                      key={category.id}
                      variant={selectedCategory === category.id ? "default" : "ghost"}
                      onClick={() => {
                        setSelectedCategory(category.id)
                        setMobileMenuOpen(false)
                      }}
                      className="w-full justify-start text-white hover:text-purple-300"
                    >
                      <Icon className="w-4 h-4 mr-2" />
                      {category.name}
                    </Button>
                  )
                })}
              </motion.nav>
            )}
          </AnimatePresence>
        </div>
      </header>

      {/* Main Content */}
      <main className="container mx-auto px-4 py-8">
        <motion.div
          initial={{ opacity: 0, y: 20 }}
          animate={{ opacity: 1, y: 0 }}
          className="text-center mb-12"
        >
          <h2 className="text-4xl md:text-6xl font-bold text-white mb-4">
            مرحباً بك في مركز الهاكات
          </h2>
          <p className="text-xl text-purple-300 max-w-2xl mx-auto">
            اكتشف أفضل الأدوات والهاكات لجميع الأنظمة
          </p>
        </motion.div>

        {/* How to Download Section */}
        {selectedCategory === 'home' && (
          <motion.div
            initial={{ opacity: 0, y: 20 }}
            animate={{ opacity: 1, y: 0 }}
            className="bg-black/40 backdrop-blur-md border-white/10 p-8 rounded-lg shadow-lg mb-12"
          >
            <h3 className="text-3xl font-bold text-white mb-6 text-center">كيفية التحميل</h3>
            <div className="space-y-6 text-gray-300 text-lg">
              <p>
                لتحميل أي هاك، اتبع الخطوات التالية:
              </p>
              <ol className="list-decimal list-inside space-y-3">
                <li>تصفح الفئات المتاحة (مثل هاكات PC).</li>
                <li>اختر الهاك الذي ترغب في تحميله.</li>
                <li>اضغط على زر <span className="font-semibold text-purple-300">تحميل الآن</span> الموجود أسفل وصف الهاك.</li>
                <li>سيتم توجيهك إلى صفحة التحميل أو سيبدأ التحميل مباشرةً.</li>
                <li>استمتع بالهاك!</li>
              </ol>
              <p className="text-center text-sm text-gray-400 mt-8">
                تنبيه: استخدم هذه الأدوات على مسؤوليتك الخاصة.
              </p>
            </div>
          </motion.div>
        )}

        {/* Hacks Grid */}
        {selectedCategory !== 'home' && getFilteredHacks().length > 0 && (
          <motion.div
            layout
            className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6"
          >
            <AnimatePresence>
              {getFilteredHacks().map((hack, index) => (
                <motion.div
                  key={hack.id}
                  layout
                  initial={{ opacity: 0, scale: 0.9 }}
                  animate={{ opacity: 1, scale: 1 }}
                  exit={{ opacity: 0, scale: 0.9 }}
                  transition={{ delay: index * 0.1 }}
                  whileHover={{ y: -5 }}
                  className="group"
                >
                  <Card className="bg-black/40 backdrop-blur-md border-white/10 hover:border-purple-500/50 transition-all duration-300 h-full">
                    <CardHeader>
                      <div className="flex items-center justify-between">
                        <CardTitle className="text-white group-hover:text-purple-300 transition-colors">
                          {hack.name}
                        </CardTitle>
                        <Badge variant="secondary" className="bg-purple-500/20 text-purple-300 border-purple-500/30">
                          {hack.status}
                        </Badge>
                      </div>
                      <CardDescription className="text-gray-300">
                        {hack.description}
                      </CardDescription>
                    </CardHeader>
                    <CardContent>
                      <div className="space-y-4">
                        <div className="flex items-center justify-between">
                          <Badge variant="outline" className="text-purple-300 border-purple-500/30">
                            {hack.category}
                          </Badge>
                        </div>
                        
                        {/* Download Button with Reaction */}
                        <motion.div
                          whileTap={{ scale: 0.95 }}
                          className="relative"
                        >
                          <Button
                            onClick={() => handleDownload(hack.downloadUrl, hack.name, hack.id)}
                            className="w-full bg-gradient-to-r from-purple-500 to-pink-500 hover:from-purple-600 hover:to-pink-600 text-white font-semibold py-2 px-4 rounded-lg transition-all duration-300 transform hover:scale-105 relative overflow-hidden"
                          >
                            <motion.div
                              initial={false}
                              animate={downloadClicks[hack.id] ? { scale: [1, 1.2, 1], rotate: [0, 10, -10, 0] } : {}}
                              transition={{ duration: 0.5 }}
                              className="flex items-center justify-center"
                            >
                              <Download className="w-4 h-4 mr-2" />
                              تحميل الآن
                            </motion.div>
                            
                            {/* عداد التحميلات */}
                            <motion.div
                              initial={{ opacity: 0, scale: 0 }}
                              animate={downloadClicks[hack.id] ? { opacity: 1, scale: 1 } : { opacity: 0, scale: 0 }}
                              className="absolute -top-2 -right-2 bg-green-500 text-white text-xs rounded-full w-6 h-6 flex items-center justify-center font-bold"
                            >
                              {getDownloadCount(hack.id)}
                            </motion.div>
                          </Button>
                          
                          {/* تأثير الجسيمات عند الضغط */}
                          <AnimatePresence>
                            {downloadClicks[hack.id] && (
                              <motion.div
                                initial={{ opacity: 1 }}
                                animate={{ opacity: 0 }}
                                exit={{ opacity: 0 }}
                                transition={{ duration: 1 }}
                                className="absolute inset-0 pointer-events-none"
                              >
                                {[...Array(6)].map((_, i) => (
                                  <motion.div
                                    key={i}
                                    initial={{ 
                                      x: "50%", 
                                      y: "50%", 
                                      scale: 0,
                                      opacity: 1
                                    }}
                                    animate={{ 
                                      x: `${50 + (Math.random() - 0.5) * 200}%`,
                                      y: `${50 + (Math.random() - 0.5) * 200}%`,
                                      scale: [0, 1, 0],
                                      opacity: [1, 1, 0]
                                    }}
                                    transition={{ 
                                      duration: 0.8,
                                      delay: i * 0.1
                                    }}
                                    className="absolute w-2 h-2 bg-purple-400 rounded-full"
                                  />
                                ))}
                              </motion.div>
                            )}
                          </AnimatePresence>
                        </motion.div>

                        {/* Reactions Section for Download */}
                        <div className="flex items-center justify-between pt-2 border-t border-white/10">
                          <span className="text-sm text-gray-400">ردود الأفعال على التحميل:</span>
                          <div className="flex items-center space-x-2">
                            {reactionTypes.map((reaction) => {
                              const Icon = reaction.icon
                              const count = getReactionCount(`download-${hack.id}`, reaction.id)
                              const isActive = userReactions[`download-${hack.id}`] === reaction.id
                              
                              return (
                                <motion.button
                                  key={reaction.id}
                                  onClick={() => handleReaction(`download-${hack.id}`, reaction.id)}
                                  className={`flex items-center space-x-1 px-2 py-1 rounded-full transition-all duration-200 ${
                                    isActive 
                                      ? `${reaction.bgColor} ${reaction.color}` 
                                      : 'bg-white/10 text-gray-400 hover:bg-white/20'
                                  }`}
                                  whileHover={{ scale: 1.05 }}
                                  whileTap={{ scale: 0.95 }}
                                  animate={isActive ? { scale: [1, 1.2, 1] } : {}}
                                  transition={{ duration: 0.3 }}
                                >
                                  <Icon className="w-3 h-3" />
                                  <span className="text-xs font-medium">{count}</span>
                                </motion.button>
                              )
                            })}
                          </div>
                        </div>

                        {/* Reactions Section (for the hack itself) */}
                        <div className="flex items-center justify-between pt-2 border-t border-white/10">
                          <span className="text-sm text-gray-400">ردود الأفعال على الهاك:</span>
                          <div className="flex items-center space-x-2">
                            {reactionTypes.map((reaction) => {
                              const Icon = reaction.icon
                              const count = getReactionCount(hack.id, reaction.id)
                              const isActive = userReactions[hack.id] === reaction.id
                              
                              return (
                                <motion.button
                                  key={reaction.id}
                                  onClick={() => handleReaction(hack.id, reaction.id)}
                                  className={`flex items-center space-x-1 px-2 py-1 rounded-full transition-all duration-200 ${
                                    isActive 
                                      ? `${reaction.bgColor} ${reaction.color}` 
                                      : 'bg-white/10 text-gray-400 hover:bg-white/20'
                                  }`}
                                  whileHover={{ scale: 1.05 }}
                                  whileTap={{ scale: 0.95 }}
                                  animate={isActive ? { scale: [1, 1.2, 1] } : {}}
                                  transition={{ duration: 0.3 }}
                                >
                                  <Icon className="w-3 h-3" />
                                  <span className="text-xs font-medium">{count}</span>
                                </motion.button>
                              )
                            })}
                          </div>
                        </div>
                      </div>
                    </CardContent>
                  </Card>
                </motion.div>
              ))}
            </AnimatePresence>
          </motion.div>
        )}

        {/* Empty State */}
        {selectedCategory !== 'home' && getFilteredHacks().length === 0 && (
          <motion.div
            initial={{ opacity: 0 }}
            animate={{ opacity: 1 }}
            className="text-center py-16"
          >
            <div className="w-24 h-24 bg-purple-500/20 rounded-full flex items-center justify-center mx-auto mb-6">
              <Monitor className="w-12 h-12 text-purple-300" />
            </div>
            <h3 className="text-2xl font-bold text-white mb-2">لا توجد هاكات متاحة</h3>
            <p className="text-gray-400">
              {selectedCategory === 'pc' 
                ? 'هاكات PC قادمة قريباً...'
                : 'لم يتم العثور على هاكات في هذه الفئة'}
            </p>
          </motion.div>
        )}

        {/* Stats Section */}
        <motion.div
          initial={{ opacity: 0, y: 20 }}
          animate={{ opacity: 1, y: 0 }}
          transition={{ delay: 0.5 }}
          className="mt-16 grid grid-cols-1 md:grid-cols-3 gap-6"
        >
          <Card className="bg-black/40 backdrop-blur-md border-white/10 text-center">
            <CardContent className="pt-6">
              <div className="text-3xl font-bold text-purple-400 mb-2">
                {hacks.iphone.length}
              </div>
              <div className="text-white">هاكات iPhone</div>
            </CardContent>
          </Card>
          <Card className="bg-black/40 backdrop-blur-md border-white/10 text-center">
            <CardContent className="pt-6">
              <div className="text-3xl font-bold text-green-400 mb-2">
                {hacks.android.length}
              </div>
              <div className="text-white">هاكات Android</div>
            </CardContent>
          </Card>
          <Card className="bg-black/40 backdrop-blur-md border-white/10 text-center">
            <CardContent className="pt-6">
              <div className="text-3xl font-bold text-blue-400 mb-2">
                {hacks.pc.length}
              </div>
              <div className="text-white">هاكات PC</div>
            </CardContent>
          </Card>
        </motion.div>
      </main>

      {/* Footer */}
      <footer className="bg-black/20 backdrop-blur-md border-t border-white/10 mt-16">
        <div className="container mx-auto px-4 py-8 text-center">
          <p className="text-gray-400">
            © 2024 ZRF. جميع الحقوق محفوظة.
          </p>
          <p className="text-sm text-gray-500 mt-2">
            تنبيه: استخدم هذه الأدوات على مسؤوليتك الخاصة
          </p>
        </div>
      </footer>
    </div>
  )
}

export default App

