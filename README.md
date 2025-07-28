<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>UConn Life Sciences Explorer</title>
    <script crossorigin src="https://unpkg.com/react@18/umd/react.production.min.js"></script>
    <script crossorigin src="https://unpkg.com/react-dom@18/umd/react-dom.production.min.js"></script>
    <script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>
    <script src="https://unpkg.com/lucide-react@latest/dist/umd/lucide-react.js"></script>
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        .loading { animation: spin 1s linear infinite; }
        @keyframes spin { from { transform: rotate(0deg); } to { transform: rotate(360deg); } }
        .sr-only { position: absolute; width: 1px; height: 1px; padding: 0; margin: -1px; overflow: hidden; clip: rect(0, 0, 0, 0); white-space: nowrap; border: 0; }
        .sr-only:focus { position: static; width: auto; height: auto; padding: inherit; margin: inherit; overflow: visible; clip: auto; white-space: normal; }
    </style>
</head>
<body>
    <div id="root">
        <div style="display: flex; justify-content: center; align-items: center; height: 100vh; font-family: Arial;">
            <div style="text-align: center;">
                <div style="width: 40px; height: 40px; border: 4px solid #f3f3f3; border-top: 4px solid #3498db; border-radius: 50%; margin: 0 auto 20px;" class="loading"></div>
                <p>Loading UConn Life Sciences Explorer...</p>
            </div>
        </div>
    </div>

    <script type="text/babel">
        // Wait for all dependencies to load
        const loadApp = () => {
            const { useState, useEffect } = React;
            const { ChevronRight, ChevronLeft, Home, BookOpen, FlaskConical, Microscope, Heart, Dna, Lightbulb, Users, ExternalLink, Search, Star, StarOff, Filter, Menu, X, ArrowUp } = lucideReact;

            const LifeSciencesExplorer = () => {
                const [currentPage, setCurrentPage] = useState('home');
                const [selectedInterest, setSelectedInterest] = useState('');
                const [searchQuery, setSearchQuery] = useState('');
                const [searchResults, setSearchResults] = useState([]);
                const [showMobileMenu, setShowMobileMenu] = useState(false);

                const interests = [
                    { id: 'molecular-bio', name: 'Molecular Biology & Genetics', icon: React.createElement(Dna, { className: "w-6 h-6" }) },
                    { id: 'biotech', name: 'Biotechnology, Engineering & Innovation', icon: React.createElement(FlaskConical, { className: "w-6 h-6" }) },
                    { id: 'healthcare', name: 'Healthcare & Medicine', icon: React.createElement(Heart, { className: "w-6 h-6" }) },
                    { id: 'research', name: 'Research & Academia', icon: React.createElement(Microscope, { className: "w-6 h-6" }) },
                    { id: 'entrepreneurship', name: 'Biotech Entrepreneurship', icon: React.createElement(Lightbulb, { className: "w-6 h-6" }) },
                    { id: 'general', name: 'General Resources', icon: React.createElement(Users, { className: "w-6 h-6" }) }
                ];

                const majors = {
                    'molecular-bio': [
                        { name: 'Molecular & Cell Biology', description: 'Study life at the cellular and molecular level', url: 'https://mcb.uconn.edu/' },
                        { name: 'Masters in Genetic Counseling', description: 'Explore heredity and genetic variation', url: 'https://geneticcounseling.uconn.edu/' },
                        { name: 'Diagnostic Genetic Sciences', description: 'Genetic testing and counseling sciences', url: 'https://alliedhealth.uconn.edu/dgs/' }
                    ],
                    'biotech': [
                        { name: 'Biomedical Engineering and Innovation', description: 'Biomedical engineering with innovation focus', url: 'https://bioinnovation.engineering.uconn.edu/?utm' },
                        { name: 'Chemical and Biomolecular Engineering', description: 'Design processes for biological materials', url: 'https://chemical-biomolecular.engineering.uconn.edu/' },
                        { name: 'Agricultural and Health Biotechnology', description: 'Agricultural biotechnology and health applications', url: 'https://www.nifa.usda.gov/about-nifa/blogs/exploring-ag-biotech-careers-university-connecticut-4-h' }
                    ],
                    'healthcare': [
                        { name: 'Nursing', description: 'Direct patient care and health promotion', url: 'https://nursing.uconn.edu/' },
                        { name: 'Pre-Medicine', description: 'Prepare for medical school', url: 'https://premed.uconn.edu/' },
                        { name: 'Master in Public Health', description: 'Population health and disease prevention', url: 'https://mph.uconn.edu/' }
                    ],
                    'research': [
                        { name: 'Ecology and Evolutionary Biology', description: 'Broad study of living organisms', url: 'https://eeb.uconn.edu/' },
                        { name: 'Bioinformatics', description: 'Computational analysis of biological data', url: 'https://computing.engineering.uconn.edu/research/areas/bio-and-medical-informatics/' }
                    ],
                    'entrepreneurship': [
                        { name: 'Biology Majors & Minors', description: 'Combine business skills with scientific knowledge', url: 'https://biology.clas.uconn.edu/undergraduate/' },
                        { name: 'Engineering + Innovation Minor', description: 'Technical skills with entrepreneurial mindset', url: 'https://engineering.uconn.edu/' }
                    ]
                };

                const resources = {
                    'molecular-bio': [
                        { name: 'Institute for Systems Genomics (ISG)', url: 'https://isg.uconn.edu/undergraduates/', description: 'Undergraduate research opportunities in genomics' },
                        { name: 'UConn Biotech Club', url: 'https://uconntact.uconn.edu/organization/uconnbiotechclub', description: 'Student organization for biotech enthusiasts' }
                    ],
                    'biotech': [
                        { name: 'Innovation Shop', url: 'https://innovationshop.engineering.uconn.edu/machine-shop/', description: 'College of engineering' },
                        { name: 'Innovation Labs', url: 'https://innovatelabs.business.uconn.edu/', description: 'School of business' },
                        { name: 'UConn Biotech Club', url: 'https://uconntact.uconn.edu/organization/uconnbiotechclub', description: 'Active student biotechnology organization' },
                        { name: 'Connecticut Center for Entrepreneurship and Innovation', url: 'https://ccei.uconn.edu/', description: 'Entrepreneurship education and support' }
                    ],
                    'healthcare': [
                        { name: 'UConn Health Center Farmington', url: 'https://ugradresearch.uconn.edu/hrp/', description: 'Clinical experience and research opportunities' },
                        { name: 'UConn Biotech Club', url: 'https://uconntact.uconn.edu/organization/uconnbiotechclub', description: 'Student organization for biotech enthusiasts' }
                    ],
                    'research': [
                        { name: 'Institute for Systems Genomics (ISG)', url: 'https://isg.uconn.edu/undergraduates/', description: 'Research opportunities in systems biology' },
                        { name: 'UConn Biotech Club', url: 'https://uconntact.uconn.edu/organization/uconnbiotechclub', description: 'Student organization for biotech enthusiasts' }
                    ],
                    'entrepreneurship': [
                        { name: 'Connecticut Center for Entrepreneurship and Innovation (CCEI)', url: 'https://ccei.uconn.edu/', description: 'Entrepreneurship education and support' },
                        { name: 'Peter J. Werth Institute for Entrepreneurship & Innovation', url: 'https://werth.institute.uconn.edu/', description: 'Innovation and startup incubation' }
                    ],
                    'general': [
                        { name: 'UConn Overview', url: 'https://uconn.edu/', description: 'Programs spanning multiple departments' },
                        { name: 'UConn Tech Park', url: 'https://techpark.uconn.edu/', description: 'Hub for cutting-edge research, industry collaboration, and innovation' }
                    ]
                };

                const performSearch = (query) => {
                    if (!query.trim()) {
                        setSearchResults([]);
                        return;
                    }
                    
                    const results = [];
                    const searchTerm = query.toLowerCase();

                    Object.entries(majors).forEach(([interestId, majorsList]) => {
                        majorsList.forEach(major => {
                            if (major.name.toLowerCase().includes(searchTerm) || major.description.toLowerCase().includes(searchTerm)) {
                                results.push({
                                    ...major,
                                    type: 'major',
                                    category: interests.find(i => i.id === interestId)?.name || 'General'
                                });
                            }
                        });
                    });

                    Object.entries(resources).forEach(([interestId, resourcesList]) => {
                        resourcesList.forEach(resource => {
                            if (resource.name.toLowerCase().includes(searchTerm) || resource.description.toLowerCase().includes(searchTerm)) {
                                results.push({
                                    ...resource,
                                    type: 'resource',
                                    category: interests.find(i => i.id === interestId)?.name || 'General'
                                });
                            }
                        });
                    });

                    setSearchResults(results);
                };

                useEffect(() => {
                    const timeoutId = setTimeout(() => {
                        performSearch(searchQuery);
                    }, 300);
                    return () => clearTimeout(timeoutId);
                }, [searchQuery]);

                const ResourceCard = ({ resource }) => (
                    React.createElement('div', { className: "bg-white p-4 rounded-lg shadow-md border-l-4 border-blue-500 hover:shadow-lg transition-shadow" },
                        React.createElement('div', { className: "flex items-start justify-between" },
                            React.createElement('div', { className: "flex-1 pr-4" },
                                React.createElement('h4', { className: "font-semibold text-gray-800 mb-2" }, resource.name),
                                React.createElement('p', { className: "text-sm text-gray-600" }, resource.description)
                            ),
                            resource.url && React.createElement('button', {
                                onClick: () => window.open(resource.url, '_blank'),
                                className: "bg-blue-500 hover:bg-blue-600 text-white p-2 rounded-lg transition-colors"
                            }, React.createElement(ExternalLink, { className: "w-4 h-4" }))
                        )
                    )
                );

                const SearchBar = ({ className = "" }) => (
                    React.createElement('div', { className: `relative ${className}` },
                        React.createElement(Search, { className: "absolute left-3 top-1/2 transform -translate-y-1/2 text-gray-400 w-5 h-5" }),
                        React.createElement('input', {
                            type: "text",
                            placeholder: "Search majors, resources, or student journeys...",
                            value: searchQuery,
                            onChange: (e) => setSearchQuery(e.target.value),
                            className: "w-full pl-10 pr-4 py-3 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-500"
                        }),
                        searchQuery && React.createElement('button', {
                            onClick: () => setSearchQuery(''),
                            className: "absolute right-3 top-1/2 transform -translate-y-1/2 text-gray-400 hover:text-gray-600"
                        }, React.createElement(X, { className: "w-4 h-4" }))
                    )
                );

                const Navigation = () => (
                    React.createElement('div', { className: "bg-white shadow-sm border-b" },
                        React.createElement('div', { className: "max-w-6xl mx-auto px-4 py-3" },
                            React.createElement('div', { className: "flex items-center justify-between" },
                                React.createElement('button', {
                                    onClick: () => setCurrentPage('home'),
                                    className: "text-xl font-bold text-blue-600 hover:text-blue-800"
                                }, "UConn Life Sciences Explorer"),
                                
                                React.createElement('div', { className: "hidden md:flex items-center space-x-4" },
                                    React.createElement(SearchBar, { className: "w-64" })
                                ),
                                
                                React.createElement('button', {
                                    onClick: () => setShowMobileMenu(!showMobileMenu),
                                    className: "md:hidden p-2 text-gray-600 hover:text-blue-600 rounded"
                                }, showMobileMenu ? React.createElement(X, { className: "w-6 h-6" }) : React.createElement(Menu, { className: "w-6 h-6" }))
                            ),
                            
                            showMobileMenu && React.createElement('div', { className: "md:hidden mt-4" },
                                React.createElement(SearchBar)
                            )
                        )
                    )
                );

                const renderHome = () => (
                    React.createElement('div', { className: "max-w-4xl mx-auto p-4 md:p-6" },
                        React.createElement('div', { className: "text-center mb-8" },
                            React.createElement('h1', { className: "text-3xl md:text-4xl font-bold text-gray-800 mb-2" }, "UConn Life Sciences & Biotech Explorer"),
                            React.createElement('p', { className: "text-sm text-gray-500 mb-4" }, "Last Updated: July 2025"),
                            React.createElement('p', { className: "text-lg text-gray-600" }, "Discover your path in life sciences and biotechnology innovation")
                        ),
                        
                        React.createElement('div', { className: "bg-yellow-50 p-4 md:p-6 rounded-lg border-l-4 border-yellow-500 mb-8" },
                            React.createElement('h3', { className: "text-lg font-semibold text-yellow-800 mb-2" }, "Disclaimer"),
                            React.createElement('p', { className: "text-sm text-yellow-700" }, 
                                "This application is an independent project and is not affiliated with or endorsed by the University of Connecticut. " +
                                "The information provided is for general guidance only and may not reflect the most current university policies, programs, or resources. " +
                                "Users should verify details with official UConn sources."
                            )
                        ),
                        
                        React.createElement('div', { className: "mb-8" },
                            React.createElement(SearchBar, { className: "max-w-2xl mx-auto" }),
                            
                            searchQuery && React.createElement('div', { className: "max-w-2xl mx-auto mt-4" },
                                React.createElement('h3', { className: "text-lg font-semibold mb-3" }, `Search Results (${searchResults.length})`),
                                React.createElement('div', { className: "space-y-3 max-h-96 overflow-y-auto" },
                                    searchResults.map((result, index) => React.createElement(ResourceCard, { key: index, resource: result })),
                                    searchResults.length === 0 && React.createElement('div', { className: "text-center py-8 text-gray-500" }, `No results found for "${searchQuery}"`)
                                )
                            )
                        ),
                        
                        React.createElement('div', { className: "grid md:grid-cols-2 gap-6 mb-8" },
                            React.createElement('div', { className: "bg-gradient-to-br from-blue-500 to-indigo-600 text-white p-6 rounded-lg shadow-lg" },
                                React.createElement('div', { className: "flex items-center mb-4" },
                                    React.createElement(BookOpen, { className: "w-8 h-8 mr-3" }),
                                    React.createElement('h2', { className: "text-2xl font-bold" }, "Explore by Interest")
                                ),
                                React.createElement('p', { className: "mb-4" }, "Find majors and resources that match your passions"),
                                React.createElement('button', {
                                    onClick: () => setCurrentPage('interests'),
                                    className: "bg-white text-blue-600 px-4 py-2 rounded-lg font-semibold hover:bg-gray-100 flex items-center"
                                },
                                    "Get Started ",
                                    React.createElement(ChevronRight, { className: "w-4 h-4 ml-2" })
                                )
                            ),
                            
                            React.createElement('div', { className: "bg-gradient-to-br from-green-500 to-teal-600 text-white p-6 rounded-lg shadow-lg" },
                                React.createElement('div', { className: "flex items-center mb-4" },
                                    React.createElement(Users, { className: "w-8 h-8 mr-3" }),
                                    React.createElement('h2', { className: "text-2xl font-bold" }, "Student Stories")
                                ),
                                React.createElement('p', { className: "mb-4" }, "Real examples of student success"),
                                React.createElement('button', {
                                    onClick: () => alert('Student stories feature coming soon!'),
                                    className: "bg-white text-green-600 px-4 py-2 rounded-lg font-semibold hover:bg-gray-100 flex items-center"
                                },
                                    "View Stories ",
                                    React.createElement(ChevronRight, { className: "w-4 h-4 ml-2" })
                                )
                            )
                        ),

                        React.createElement('div', { className: "bg-gray-50 p-6 rounded-lg" },
                            React.createElement('h3', { className: "text-xl font-bold text-gray-800 mb-4" }, "Why Choose UConn for Life Sciences?"),
                            React.createElement('div', { className: "grid md:grid-cols-3 gap-4" },
                                React.createElement('div', { className: "text-center" },
                                    React.createElement(Microscope, { className: "w-12 h-12 mx-auto text-blue-500 mb-2" }),
                                    React.createElement('h4', { className: "font-semibold" }, "World-Class Research"),
                                    React.createElement('p', { className: "text-sm text-gray-600" }, "Access to cutting-edge facilities and faculty")
                                ),
                                React.createElement('div', { className: "text-center" },
                                    React.createElement(Lightbulb, { className: "w-12 h-12 mx-auto text-green-500 mb-2" }),
                                    React.createElement('h4', { className: "font-semibold" }, "Innovation Ecosystem"),
                                    React.createElement('p', { className: "text-sm text-gray-600" }, "Entrepreneur programs and startup support")
                                ),
                                React.createElement('div', { className: "text-center" },
                                    React.createElement(Heart, { className: "w-12 h-12 mx-auto text-red-500 mb-2" }),
                                    React.createElement('h4', { className: "font-semibold" }, "Healthcare Connection"),
                                    React.createElement('p', { className: "text-sm text-gray-600" }, "Direct links to UConn Health and clinical experience")
                                )
                            )
                        )
                    )
                );

                const renderInterests = () => (
                    React.createElement('div', { className: "max-w-4xl mx-auto p-4 md:p-6" },
                        React.createElement('div', { className: "flex items-center mb-6" },
                            React.createElement('button', { 
                                onClick: () => setCurrentPage('home'), 
                                className: "mr-4 p-2 hover:bg-gray-100 rounded-lg" 
                            }, React.createElement(Home, { className: "w-6 h-6" })),
                            React.createElement('h1', { className: "text-2xl md:text-3xl font-bold text-gray-800" }, "Choose Your Interest Area")
                        ),
                        
                        React.createElement('div', { className: "grid sm:grid-cols-2 lg:grid-cols-3 gap-4" },
                            interests.map((interest) =>
                                React.createElement('button', {
                                    key: interest.id,
                                    onClick: () => {
                                        setSelectedInterest(interest.id);
                                        setCurrentPage('details');
                                    },
                                    className: "bg-white p-6 rounded-lg shadow-md hover:shadow-lg transition-all duration-200 border-2 border-transparent hover:border-blue-500 text-left"
                                },
                                    React.createElement('div', { className: "flex items-center mb-3" },
                                        React.createElement('div', { className: "text-blue-500 mr-3" }, interest.icon),
                                        React.createElement('h3', { className: "text-lg font-semibold text-gray-800" }, interest.name)
                                    ),
                                    React.createElement(ChevronRight, { className: "w-5 h-5 text-gray-400" })
                                )
                            )
                        )
                    )
                );

                const renderDetails = () => {
                    const selectedInterestData = interests.find(i => i.id === selectedInterest);
                    const relevantMajors = majors[selectedInterest] || [];
                    const relevantResources = resources[selectedInterest] || [];

                    return React.createElement('div', { className: "max-w-6xl mx-auto p-4 md:p-6" },
                        React.createElement('div', { className: "flex items-center mb-6" },
                            React.createElement('button', { 
                                onClick: () => setCurrentPage('interests'), 
                                className: "mr-4 p-2 hover:bg-gray-100 rounded-lg" 
                            }, React.createElement(ChevronLeft, { className: "w-6 h-6" })),
                            React.createElement('button', { 
                                onClick: () => setCurrentPage('home'), 
                                className: "mr-4 p-2 hover:bg-gray-100 rounded-lg" 
                            }, React.createElement(Home, { className: "w-6 h-6" })),
                            React.createElement('div', { className: "flex items-center" },
                                React.createElement('div', { className: "text-blue-500 mr-3" }, selectedInterestData?.icon),
                                React.createElement('h1', { className: "text-2xl md:text-3xl font-bold text-gray-800" }, selectedInterestData?.name)
                            )
                        ),
                        
                        selectedInterest === 'general' ? 
                            React.createElement('div', { className: "max-w-4xl" },
                                React.createElement('h2', { className: "text-2xl font-bold text-gray-800 mb-4" }, "Supporting Resources"),
                                React.createElement('div', { className: "space-y-4" },
                                    relevantResources.map((resource, index) => React.createElement(ResourceCard, { key: index, resource }))
                                )
                            ) :
                            React.createElement('div', { className: "grid lg:grid-cols-2 gap-8" },
                                React.createElement('div', {},
                                    React.createElement('h2', { className: "text-2xl font-bold text-gray-800 mb-4" }, "Areas of Academic Focus"),
                                    React.createElement('div', { className: "space-y-4" },
                                        relevantMajors.map((major, index) => React.createElement(ResourceCard, { key: index, resource: major }))
                                    )
                                ),
                                
                                React.createElement('div', {},
                                    React.createElement('h2', { className: "text-2xl font-bold text-gray-800 mb-4" }, "Supporting Resources"),
                                    React.createElement('div', { className: "space-y-4" },
                                        relevantResources.map((resource, index) => React.createElement(ResourceCard, { key: index, resource }))
                                    )
                                )
                            )
                    );
                };

                return React.createElement('div', { className: "min-h-screen bg-gradient-to-br from-blue-50 to-indigo-100" },
                    React.createElement(Navigation),
                    React.createElement('main', { className: "pb-20" },
                        currentPage === 'home' && renderHome(),
                        currentPage === 'interests' && renderInterests(),
                        currentPage === 'details' && renderDetails()
                    )
                );
            };

            ReactDOM.render(React.createElement(LifeSciencesExplorer), document.getElementById('root'));
        };

        // Wait for all scripts to load
        setTimeout(loadApp, 2000);
    </script>
</body>
</html>
