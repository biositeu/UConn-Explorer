<div className="flex flex-col gap-2">
                            {resource.url && (
                                <button 
                                    onClick={() => window.open(resource.url, '_blank')}
                                    className="bg-blue-500 hover:bg-blue-600 text-white p-2 rounded-lg transition-colors"
                                    aria-label="Open external link"
                                >
                                    🔗
                                </button>
                            )}
                        </div>
                    </div>
                </div>
            );

            // Enhanced case study card
            const CaseStudyCard = ({ study }) => (
                <div className={`p-6 rounded-lg shadow-md ${study.isReal ? 'bg-gradient-to-br from-green-50 to-emerald-50 border-l-4 border-green-500' : 'bg-gradient-to-br from-blue-50 to-indigo-50 border-l-4 border-blue-500'}`}>
                    <div className="flex items-start justify-between mb-2">
                        <div className="flex-1 pr-4">
                            <div className="flex items-center gap-2 mb-2">
                                <h3 className="text-xl font-bold text-gray-800">{study.title}</h3>
                                <span className={`text-xs px-2 py-1 rounded ${study.isReal ? 'bg-green-100 text-green-800' : 'bg-blue-100 text-blue-800'}`}>
                                    {study.isReal ? 'Real Story' : 'Illustrative'}
                                </span>
                            </div>
                        </div>
                        {study.url && (
                            <button 
                                onClick={() => window.open(study.url, '_blank')}
                                className="bg-blue-500 hover:bg-blue-600 text-white p-2 rounded-lg"
                            >
                                🔗
                            </button>
                        )}
                    </div>
                    <div className="mb-3">
                        <span className="inline-block bg-blue-100 text-blue-800 px-3 py-1 rounded-full text-sm font-medium mr-2">
                            {study.major}
                        </span>
                        <span className="inline-block bg-green-100 text-green-800 px-3 py-1 rounded-full text-sm font-medium">
                            {study.minor}
                        </span>
                    </div>
                    <p className="text-gray-700 mb-4">{study.story}</p>
                    <div className="mb-4">
                        <h4 className="font-semibold text-gray-800 mb-2">Key Resources:</h4>
                        <div className="flex flex-wrap gap-2">
                            {study.resources.map((resource, index) => (
                                <span key={index} className="bg-yellow-100 text-yellow-800 px-2 py-1 rounded text-xs">
                                    {resource}
                                </span>
                            ))}
                        </div>
                    </div>
                    <div className="bg-green-50 p-3 rounded-lg">
                        <p className="text-sm font-medium text-green-800">Outcome:</p>
                        <p className="text-sm text-green-700">{study.outcome}</p>
                    </div>
                </div>
            );

            // Enhanced navigation with mobile menu
            const Navigation = () => (
                <div className="bg-white shadow-sm border-b">
                    <div className="max-w-6xl mx-auto px-4 py-3">
                        <div className="flex items-center justify-between">
                            <button 
                                onClick={() => setCurrentPage('home')}
                                className="text-xl font-bold text-blue-600 hover:text-blue-800 focus:outline-none focus:ring-2 focus:ring-blue-500 rounded"
                            >
                                UConn Life Sciences Explorer
                            </button>
                            
                            {/* Desktop menu */}
                            <div className="hidden md:flex items-center space-x-4">
                                <SearchBar className="w-64" />
                            </div>
                            
                            {/* Mobile menu button */}
                            <button 
                                onClick={() => setShowMobileMenu(!showMobileMenu)}
                                className="md:hidden p-2 text-gray-600 hover:text-blue-600 rounded"
                            >
                                {showMobileMenu ? '✕' : '☰'}
                            </button>
                        </div>
                        
                        {/* Mobile menu */}
                        {showMobileMenu && (
                            <div className="md:hidden mt-4 space-y-4">
                                <SearchBar />
                                {compareList.length > 0 && (
                                    <div className="bg-blue-100 text-blue-800 px-3 py-2 rounded-lg text-sm">
                                        Compare ({compareList.length})
                                    </div>
                                )}
                            </div>
                        )}
                    </div>
                </div>
            );

            const renderHome = () => (
                <div className="max-w-4xl mx-auto p-4 md:p-6">
                    <div className="text-center mb-8">
                        <h1 className="text-3xl md:text-4xl font-bold text-gray-800 mb-2">UConn Life Sciences & Biotech Explorer</h1>
                        <p className="text-sm text-gray-500 mb-4">Last Updated: July 2025</p>
                        <p className="text-lg text-gray-600">Discover your path in life sciences and biotechnology innovation</p>
                    </div>
                    
                    <div className="bg-yellow-50 p-4 md:p-6 rounded-lg border-l-4 border-yellow-500 mb-8">
                        <h3 className="text-lg font-semibold text-yellow-800 mb-2">Disclaimer</h3>
                        <p className="text-sm text-yellow-700">
                            This application is an independent project and is not affiliated with or endorsed by the University of Connecticut. 
                            The information provided is for general guidance only and may not reflect the most current university policies, programs, or resources. 
                            Users should verify details with official UConn sources. The developer assumes no responsibility for any errors, omissions, 
                            or outcomes resulting from the use of this application.
                        </p>
                    </div>
                    
                    <div className="mb-8">
                        <SearchBar className="max-w-2xl mx-auto" />
                        
                        {searchQuery && (
                            <div className="max-w-2xl mx-auto mt-4">
                                <div className="flex items-center justify-between mb-3">
                                    <h3 className="text-lg font-semibold">Search Results ({searchResults.length})</h3>
                                    <div className="flex items-center gap-2">
                                        <span className="text-gray-400">🔧</span>
                                        <select 
                                            value={filterType} 
                                            onChange={(e) => setFilterType(e.target.value)}
                                            className="text-sm border border-gray-300 rounded px-2 py-1 focus:outline-none focus:ring-2 focus:ring-blue-500"
                                        >
                                            <option value="all">All Types</option>
                                            <option value="major">Majors</option>
                                            <option value="program">Programs</option>
                                            <option value="club">Clubs</option>
                                            <option value="facility">Facilities</option>
                                            <option value="research">Research</option>
                                        </select>
                                    </div>
                                </div>
                                <div className="space-y-3 max-h-96 overflow-y-auto">
                                    {searchResults.map((result, index) => (
                                        <ResourceCard key={index} resource={result} />
                                    ))}
                                    {searchResults.length === 0 && (
                                        <div className="text-center py-8 text-gray-500">
                                            No results found for "{searchQuery}"
                                        </div>
                                    )}
                                </div>
                            </div>
                        )}
                    </div>
                    
                    <div className="grid md:grid-cols-2 gap-6 mb-8">
                        <div className="bg-gradient-to-br from-blue-500 to-indigo-600 text-white p-6 rounded-lg shadow-lg">
                            <div className="flex items-center mb-4">
                                <span className="text-3xl mr-3">📚</span>
                                <h2 className="text-2xl font-bold">Explore by Interest</h2>
                            </div>
                            <p className="mb-4">Find majors and resources that match your passions</p>
                            <button 
                                onClick={() => setCurrentPage('interests')}
                                className="bg-white text-blue-600 px-4 py-2 rounded-lg font-semibold hover:bg-gray-100 flex items-center transition-colors focus:outline-none focus:ring-2 focus:ring-white"
                            >
                                Get Started <span className="ml-2">→</span>
                            </button>
                        </div>
                        
                        <div className="bg-gradient-to-br from-green-500 to-teal-600 text-white p-6 rounded-lg shadow-lg">
                            <div className="flex items-center mb-4">
                                <span className="text-3xl mr-3">👥</span>
                                <h2 className="text-2xl font-bold">Student Journeys</h2>
                            </div>
                            <p className="mb-4">Real stories and illustrative examples of student success</p>
                            <button 
                                onClick={() => setCurrentPage('case-studies')}
                                className="bg-white text-green-600 px-4 py-2 rounded-lg font-semibold hover:bg-gray-100 flex items-center transition-colors focus:outline-none focus:ring-2 focus:ring-white"
                            >
                                View Stories <span className="ml-2">→</span>
                            </button>
                        </div>
                    </div>
                    
                    {favorites.size > 0 && (
                        <div className="bg-yellow-50 p-6 rounded-lg mb-8">
                            <h3 className="text-xl font-bold text-gray-800 mb-4 flex items-center gap-2">
                                <span className="text-yellow-500">⭐</span>
                                Your Favorites ({favorites.size})
                            </h3>
                            <p className="text-gray-600">You've saved {favorites.size} item{favorites.size !== 1 ? 's' : ''} to explore later!</p>
                        </div>
                    )}
                    
                    <div className="bg-gray-50 p-6 rounded-lg">
                        <h3 className="text-xl font-bold text-gray-800 mb-4">Why Choose UConn for Life Sciences?</h3>
                        <div className="grid md:grid-cols-3 gap-4">
                            <div className="text-center">
                                <div className="text-4xl mb-2">🔬</div>
                                <h4 className="font-semibold">World-Class Research</h4>
                                <p className="text-sm text-gray-600">Access to cutting-edge facilities and faculty</p>
                            </div>
                            <div className="text-center">
                                <div className="text-4xl mb-2">💡</div>
                                <h4 className="font-semibold">Innovation Ecosystem</h4>
                                <p className="text-sm text-gray-600">Entrepreneur programs and startup support</p>
                            </div>
                            <div className="text-center">
                                <div className="text-4xl mb-2">❤️</div>
                                <h4 className="font-semibold">Healthcare Connection</h4>
                                <p className="text-sm text-gray-600">Direct links to UConn Health and clinical experience</p>
                            </div>
                        </div>
                    </div>
                </div>
            );

            const renderInterests = () => (
                <div className="max-w-4xl mx-auto p-4 md:p-6">
                    <Breadcrumb />
                    <h1 className="text-2xl md:text-3xl font-bold text-gray-800 mb-6">Choose Your Interest Area</h1>
                    
                    <div className="grid sm:grid-cols-2 lg:grid-cols-3 gap-4">
                        {interests.map((interest) => (
                            <button
                                key={interest.id}
                                onClick={() => {
                                    setSelectedInterest(interest.id);
                                    setCurrentPage('details');
                                }}
                                className="bg-white p-6 rounded-lg shadow-md hover:shadow-lg transition-all duration-200 border-2 border-transparent hover:border-blue-500 text-left focus:outline-none focus:ring-2 focus:ring-blue-500"
                            >
                                <div className="flex items-center mb-3">
                                    <div className="text-blue-500 mr-3 text-2xl">
                                        {interest.icon}
                                    </div>
                                    <h3 className="text-lg font-semibold text-gray-800">{interest.name}</h3>
                                </div>
                                <span className="text-gray-400">→</span>
                            </button>
                        ))}
                    </div>
                </div>
            );

            const renderDetails = () => {
                const selectedInterestData = interests.find(i => i.id === selectedInterest);
                const relevantMajors = majors[selectedInterest] || [];
                const relevantResources = resources[selectedInterest] || [];

                return (
                    <div className="max-w-6xl mx-auto p-4 md:p-6">
                        <Breadcrumb />
                        
                        <div className="flex items-center mb-6">
                            <div className="text-blue-500 mr-3 text-3xl">
                                {selectedInterestData?.icon}
                            </div>
                            <h1 className="text-2xl md:text-3xl font-bold text-gray-800">{selectedInterestData?.name}</h1>
                        </div>

                        {selectedInterest === 'general' ? (
                            <div className="max-w-4xl">
                                <h2 className="text-2xl font-bold text-gray-800 mb-4">Supporting Resources</h2>
                                <div className="space-y-4">
                                    {relevantResources.map((resource, index) => (
                                        <ResourceCard key={index} resource={resource} />
                                    ))}
                                </div>
                            </div>
                        ) : (
                            <div className="grid lg:grid-cols-2 gap-8">
                                <div>
                                    <h2 className="text-2xl font-bold text-gray-800 mb-4">Areas of Academic Focus</h2>
                                    <div className="space-y-4">
                                        {relevantMajors.map((major, index) => (
                                            <ResourceCard key={index} resource={major} />
                                        ))}
                                    </div>
                                </div>
                                
                                <div>
                                    <h2 className="text-2xl font-bold text-gray-800 mb-4">Supporting Resources</h2>
                                    <div className="space-y-4">
                                        {relevantResources.map((resource, index) => (
                                            <ResourceCard key={index} resource={resource} />
                                        ))}
                                    </div>
                                </div>
                            </div>
                        )}
                        
                        {compareList.length > 0 && (
                            <div className="mt-8 bg-blue-50 p-6 rounded-lg">
                                <h3 className="text-xl font-bold text-blue-800 mb-4">Compare Selected Items ({compareList.length}/3)</h3>
                                <div className="grid md:grid-cols-3 gap-4 mb-4">
                                    {compareList.map((item, index) => (
                                        <div key={index} className="bg-white p-3 rounded border">
                                            <div className="flex justify-between items-start mb-2">
                                                <h4 className="font-semibold text-sm">{item.name}</h4>
                                                <button 
                                                    onClick={() => toggleCompare(item)}
                                                    className="text-red-500 hover:text-red-700"
                                                >
                                                    ✕
                                                </button>
                                            </div>
                                            <p className="text-xs text-gray-600">{item.description}</p>
                                            <div className="flex gap-1 mt-2">
                                                <span className="text-xs bg-gray-100 px-2 py-1 rounded">{item.type}</span>
                                                <span className="text-xs bg-blue-100 px-2 py-1 rounded">{item.department}</span>
                                            </div>
                                        </div>
                                    ))}
                                </div>
                                <button 
                                    onClick={() => setCompareList([])}
                                    className="text-blue-600 hover:text-blue-800 text-sm font-medium"
                                >
                                    Clear All
                                </button>
                            </div>
                        )}
                        
                        <div className="mt-8 bg-blue-50 p-6 rounded-lg">
                            <h3 className="text-xl font-bold text-blue-800 mb-2">Next Steps</h3>
                            <p className="text-blue-700 mb-4">Ready to explore more? Check out our student journeys to see how students combined different majors and resources to achieve their goals.</p>
                            <button 
                                onClick={() => setCurrentPage('case-studies')}
                                className="bg-blue-600 text-white px-4 py-2 rounded-lg font-semibold hover:bg-blue-700 transition-colors focus:outline-none focus:ring-2 focus:ring-blue-500"
                            >
                                View Student Journeys
                            </button>
                        </div>
                    </div>
                );
            };

            const renderCaseStudies = () => (
                <div className="max-w-6xl mx-auto p-4 md:p-6">
                    <Breadcrumb />
                    <h1 className="text-2xl md:text-3xl font-bold text-gray-800 mb-6">Student Stories</h1>

                    <div className="mb-8">
                        <div className="flex items-center gap-4 mb-4">
                            <h2 className="text-2xl font-bold text-gray-800">Real Student Success Stories</h2>
                            <span className="bg-green-100 text-green-800 px-3 py-1 rounded-full text-sm font-medium">
                                Verified Stories
                            </span>
                        </div>
                        <div className="grid lg:grid-cols-2 gap-6">
                            {realStudentStories.map((story, index) => (
                                <CaseStudyCard key={index} study={story} />
                            ))}
                        </div>
                    </div>

                    <div className="mb-8">
                        <div className="flex items-center gap-4 mb-4">
                            <h2 className="text-2xl font-bold text-gray-800">Illustrative Student Journeys</h2>
                            <span className="bg-blue-100 text-blue-800 px-3 py-1 rounded-full text-sm font-medium">
                                Example Paths
                            </span>
                        </div>
                        <div className="mb-6 p-4 bg-blue-50 rounded-lg border border-blue-200">
                            <p className="text-sm text-blue-700">
                                <strong>The following scenarios represent illustrative journeys of students exploring different majors and the campus resources that help them reach their goals. While these examples are fictional, they are designed to reflect common experiences and challenges students face.</strong>
                            </p>
                        </div>
                        <div className="grid lg:grid-cols-2 gap-6">
                            {illustrativeJourneys.map((journey, index) => (
                                <CaseStudyCard key={index} study={journey} />
                            ))}
                        </div>
                    </div>
                    
                    <div className="mt-8 bg-green-50 p-6 rounded-lg text-center">
                        <h3 className="text-xl font-bold text-green-800 mb-2">Ready to Start Your Journey?</h3>
                        <p className="text-green-700 mb-4">Explore different interest areas to find the perfect combination of majors and resources for your goals.</p>
                        <button 
                            onClick={() => setCurrentPage('interests')}
                            className="bg-green-600 text-white px-6 py-3 rounded-lg font-semibold hover:bg-green-700 transition-colors focus:outline-none focus:ring-2 focus:ring-green-500"
                        >
                            Explore by Interest
                        </button>
                    </div>
                </div>
            );

            return (
                <div className="min-h-screen bg-gradient-to-br from-blue-50 to-indigo-100">
                    <Navigation />
                    
                    <main className="pb-20" id="main-content">
                        {currentPage === 'home' && renderHome()}
                        {currentPage === 'interests' && renderInterests()}
                        {currentPage === 'details' && renderDetails()}
                        {currentPage === 'case-studies' && renderCaseStudies()}
                    </main>

                    {/* Scroll to top button */}
                    {showScrollTop && (
                        <button
                            onClick={scrollToTop}
                            className="fixed bottom-6 right-6 bg-blue-600 text-white p-3 rounded-full shadow-lg hover:bg-blue-700 transition-colors focus:outline-none focus:ring-2 focus:ring-blue-500 z-40"
                            aria-label="Scroll to top"
                        >
                            ↑
                        </button>
                    )}
                </div>
            );
        };

        // Wait for all dependencies to load before rendering
        setTimeout(() => {
            ReactDOM.render(React.createElement(LifeSciencesExplorer), document.getElementById('root'));
        }, 1000);
    </script>
</body>
</html>
