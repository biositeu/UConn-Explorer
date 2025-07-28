<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>UConn Life Sciences Explorer</title>
    <script crossorigin src="https://unpkg.com/react@18/umd/react.production.min.js"></script>
    <script crossorigin src="https://unpkg.com/react-dom@18/umd/react-dom.production.min.js"></script>
    <script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        @keyframes spin { from { transform: rotate(0deg); } to { transform: rotate(360deg); } }
        .loading { animation: spin 1s linear infinite; }
    </style>
</head>
<body>
    <div id="root">
        <div style="display: flex; justify-content: center; align-items: center; height: 100vh; font-family: Arial; background: linear-gradient(135deg, #dbeafe, #e0e7ff);">
            <div style="text-align: center;">
                <div style="width: 40px; height: 40px; border: 4px solid #f3f3f3; border-top: 4px solid #3498db; border-radius: 50%; margin: 0 auto 20px;" class="loading"></div>
                <p>Loading UConn Life Sciences Explorer...</p>
            </div>
        </div>
    </div>

    <script type="text/babel">
        const { useState, useEffect } = React;

        const LifeSciencesExplorer = () => {
            const [currentPage, setCurrentPage] = useState('home');
            const [selectedInterest, setSelectedInterest] = useState('');
            const [searchQuery, setSearchQuery] = useState('');
            const [searchResults, setSearchResults] = useState([]);

            const interests = [
                { id: 'molecular-bio', name: 'Molecular Biology & Genetics', icon: '🧬' },
                { id: 'biotech', name: 'Biotechnology, Engineering & Innovation', icon: '🔬' },
                { id: 'healthcare', name: 'Healthcare & Medicine', icon: '❤️' },
                { id: 'research', name: 'Research & Academia', icon: '🔬' },
                { id: 'entrepreneurship', name: 'Biotech Entrepreneurship', icon: '💡' },
                { id: 'general', name: 'General Resources', icon: '👥' }
            ];

            const data = {
                'molecular-bio': {
                    majors: [
                        { name: 'Molecular & Cell Biology', description: 'Study life at the cellular and molecular level', url: 'https://mcb.uconn.edu/' },
                        { name: 'Genetic Counseling', description: 'Explore heredity and genetic variation', url: 'https://geneticcounseling.uconn.edu/' },
                        { name: 'Diagnostic Genetic Sciences', description: 'Genetic testing and counseling sciences', url: 'https://alliedhealth.uconn.edu/dgs/' }
                    ],
                    resources: [
                        { name: 'Institute for Systems Genomics', description: 'Undergraduate research opportunities', url: 'https://isg.uconn.edu/undergraduates/' },
                        { name: 'UConn Biotech Club', description: 'Student organization for biotech enthusiasts', url: 'https://uconntact.uconn.edu/organization/uconnbiotechclub' }
                    ]
                },
                'biotech': {
                    majors: [
                        { name: 'Biomedical Engineering and Innovation', description: 'Biomedical engineering with innovation focus', url: 'https://bioinnovation.engineering.uconn.edu/?utm' },
                        { name: 'Chemical and Biomolecular Engineering', description: 'Design processes for biological materials', url: 'https://chemical-biomolecular.engineering.uconn.edu/' },
                        { name: 'Agricultural and Health Biotechnology', description: 'Agricultural biotechnology applications', url: 'https://www.nifa.usda.gov/about-nifa/blogs/exploring-ag-biotech-careers-university-connecticut-4-h' }
                    ],
                    resources: [
                        { name: 'Innovation Shop', description: 'College of engineering maker space', url: 'https://innovationshop.engineering.uconn.edu/machine-shop/' },
                        { name: 'Innovation Labs', description: 'School of business innovation space', url: 'https://innovatelabs.business.uconn.edu/' },
                        { name: 'CCEI', description: 'Entrepreneurship education and support', url: 'https://ccei.uconn.edu/' }
                    ]
                },
                'healthcare': {
                    majors: [
                        { name: 'Nursing', description: 'Direct patient care and health promotion', url: 'https://nursing.uconn.edu/' },
                        { name: 'Pre-Medicine', description: 'Prepare for medical school', url: 'https://premed.uconn.edu/' },
                        { name: 'Public Health', description: 'Population health and disease prevention', url: 'https://mph.uconn.edu/' }
                    ],
                    resources: [
                        { name: 'UConn Health Center', description: 'Clinical experience opportunities', url: 'https://ugradresearch.uconn.edu/hrp/' },
                        { name: 'UConn Biotech Club', description: 'Student organization', url: 'https://uconntact.uconn.edu/organization/uconnbiotechclub' }
                    ]
                },
                'research': {
                    majors: [
                        { name: 'Ecology and Evolutionary Biology', description: 'Study of living organisms', url: 'https://eeb.uconn.edu/' },
                        { name: 'Bioinformatics', description: 'Computational analysis of biological data', url: 'https://computing.engineering.uconn.edu/research/areas/bio-and-medical-informatics/' }
                    ],
                    resources: [
                        { name: 'Institute for Systems Genomics', description: 'Research opportunities', url: 'https://isg.uconn.edu/undergraduates/' },
                        { name: 'Jackson Labs', description: 'Genetics research institution', url: 'https://www.jax.org/education-and-learning/high-school-students-and-undergraduates/' }
                    ]
                },
                'entrepreneurship': {
                    majors: [
                        { name: 'Biology + Business', description: 'Combine scientific knowledge with business', url: 'https://biology.clas.uconn.edu/undergraduate/' },
                        { name: 'Engineering + Innovation', description: 'Technical skills with entrepreneurship', url: 'https://engineering.uconn.edu/' }
                    ],
                    resources: [
                        { name: 'CCEI', description: 'Entrepreneurship education and support', url: 'https://ccei.uconn.edu/' },
                        { name: 'Werth Institute', description: 'Innovation and startup incubation', url: 'https://werth.institute.uconn.edu/' }
                    ]
                },
                'general': {
                    majors: [],
                    resources: [
                        { name: 'UConn Tech Park', description: 'Research and industry collaboration hub', url: 'https://techpark.uconn.edu/' },
                        { name: 'NetWerx Alumni Mentorship', description: 'Connect with alumni mentors', url: 'https://today.uconn.edu/2025/01/new-netwerx-initiative-brings-alumni-mentorship-into-the-classroom/' }
                    ]
                }
            };

            const studentStories = [
                { title: 'Maya Patel - Genetic Sciences', major: 'Diagnostic Genetic Sciences', story: 'Being part of DGS opened my eyes to genetics in healthcare', outcome: 'Genetic counseling career prep', url: 'https://genomics.institute.uconn.edu/', isReal: true },
                { title: 'Tyler & Lyla - Pharmacy Research', major: 'Pharmacy', story: 'Two students received research awards for innovative pharmaceutical work', outcome: 'Advancing pharmaceutical science', url: 'https://today.uconn.edu/2024/05/two-school-of-pharmacy-students/', isReal: true },
                { title: 'Reza Amin - Healthcare Entrepreneur', major: 'Engineering PhD', story: 'Founded Bastion Health, serving 120,000 patients with AI diagnostics', outcome: 'Multi-million dollar healthcare company CEO', url: 'https://today.uconn.edu/2025/06/uconn-entrepreneur/', isReal: true },
                { title: 'Alex - Healthcare Innovation', major: 'Biomedical Engineering', story: 'Combined engineering with business skills for AI diagnostic tools', outcome: 'Startup founder', isReal: false }
            ];

            const performSearch = (query) => {
                if (!query.trim()) { setSearchResults([]); return; }
                const results = [];
                const searchTerm = query.toLowerCase();
                
                Object.entries(data).forEach(([interestId, content]) => {
                    [...content.majors, ...content.resources].forEach(item => {
                        if (item.name.toLowerCase().includes(searchTerm) || item.description.toLowerCase().includes(searchTerm)) {
                            results.push({ ...item, category: interests.find(i => i.id === interestId)?.name });
                        }
                    });
                });
                setSearchResults(results);
            };

            useEffect(() => {
                const timeoutId = setTimeout(() => performSearch(searchQuery), 300);
                return () => clearTimeout(timeoutId);
            }, [searchQuery]);

            const ResourceCard = ({ item }) => (
                <div className="bg-white p-4 rounded-lg shadow-md border-l-4 border-blue-500 hover:shadow-lg transition-shadow">
                    <div className="flex justify-between items-start">
                        <div className="flex-1 pr-4">
                            <h4 className="font-semibold text-gray-800 mb-2">{item.name}</h4>
                            <p className="text-sm text-gray-600">{item.description}</p>
                        </div>
                        {item.url && (
                            <button onClick={() => window.open(item.url, '_blank')} className="bg-blue-500 hover:bg-blue-600 text-white p-2 rounded-lg">
                                🔗
                            </button>
                        )}
                    </div>
                </div>
            );

            const StoryCard = ({ story }) => (
                <div className={`p-6 rounded-lg shadow-md ${story.isReal ? 'bg-green-50 border-l-4 border-green-500' : 'bg-blue-50 border-l-4 border-blue-500'}`}>
                    <div className="flex justify-between items-start mb-2">
                        <h3 className="text-xl font-bold text-gray-800 flex-1">{story.title}</h3>
                        <span className={`text-xs px-2 py-1 rounded ml-2 ${story.isReal ? 'bg-green-100 text-green-800' : 'bg-blue-100 text-blue-800'}`}>
                            {story.isReal ? 'Real Story' : 'Example'}
                        </span>
                        {story.url && (
                            <button onClick={() => window.open(story.url, '_blank')} className="bg-blue-500 hover:bg-blue-600 text-white p-2 rounded-lg ml-2">
                                🔗
                            </button>
                        )}
                    </div>
                    <div className="mb-3">
                        <span className="bg-blue-100 text-blue-800 px-3 py-1 rounded-full text-sm font-medium">{story.major}</span>
                    </div>
                    <p className="text-gray-700 mb-4">{story.story}</p>
                    <div className="bg-green-50 p-3 rounded-lg">
                        <p className="text-sm font-medium text-green-800">Outcome: {story.outcome}</p>
                    </div>
                </div>
            );

            const Navigation = () => (
                <div className="bg-white shadow-sm border-b">
                    <div className="max-w-6xl mx-auto px-4 py-3">
                        <button onClick={() => setCurrentPage('home')} className="text-xl font-bold text-blue-600 hover:text-blue-800">
                            UConn Life Sciences Explorer
                        </button>
                    </div>
                </div>
            );

            const Breadcrumb = () => {
                const pages = [];
                if (currentPage === 'home') pages.push({ name: 'Home', active: true });
                else if (currentPage === 'interests') {
                    pages.push({ name: 'Home', onClick: () => setCurrentPage('home') });
                    pages.push({ name: 'Interests', active: true });
                } else if (currentPage === 'details') {
                    pages.push({ name: 'Home', onClick: () => setCurrentPage('home') });
                    pages.push({ name: 'Interests', onClick: () => setCurrentPage('interests') });
                    pages.push({ name: interests.find(i => i.id === selectedInterest)?.name || 'Details', active: true });
                } else if (currentPage === 'stories') {
                    pages.push({ name: 'Home', onClick: () => setCurrentPage('home') });
                    pages.push({ name: 'Student Stories', active: true });
                }

                return (
                    <nav className="flex mb-4">
                        {pages.map((page, index) => (
                            <div key={index} className="flex items-center">
                                {index > 0 && <span className="mx-2 text-gray-400">→</span>}
                                {page.active ? (
                                    <span className="text-gray-500 text-sm font-medium">{page.name}</span>
                                ) : (
                                    <button onClick={page.onClick} className="text-blue-600 text-sm font-medium hover:text-blue-800">
                                        {page.name}
                                    </button>
                                )}
                            </div>
                        ))}
                    </nav>
                );
            };

            const renderHome = () => (
                <div className="max-w-4xl mx-auto p-6">
                    <div className="text-center mb-8">
                        <h1 className="text-4xl font-bold text-gray-800 mb-2">UConn Life Sciences & Biotech Explorer</h1>
                        <p className="text-sm text-gray-500 mb-4">Last Updated: July 2025</p>
                        <p className="text-lg text-gray-600">Discover your path in life sciences and biotechnology innovation</p>
                    </div>
                    
                    <div className="bg-yellow-50 p-6 rounded-lg border-l-4 border-yellow-500 mb-8">
                        <h3 className="text-lg font-semibold text-yellow-800 mb-2">Disclaimer</h3>
                        <p className="text-sm text-yellow-700">This application is an independent project and is not affiliated with or endorsed by the University of Connecticut. The information provided is for general guidance only and may not reflect the most current university policies, programs, or resources. Users should verify details with official UConn sources.</p>
                    </div>
                    
                    <div className="mb-8">
                        <div className="relative max-w-2xl mx-auto">
                            <span className="absolute left-3 top-1/2 transform -translate-y-1/2 text-gray-400">🔍</span>
                            <input
                                type="text"
                                placeholder="Search majors, resources, or student journeys..."
                                value={searchQuery}
                                onChange={(e) => setSearchQuery(e.target.value)}
                                className="w-full pl-10 pr-4 py-3 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-500"
                            />
                        </div>
                        
                        {searchQuery && (
                            <div className="max-w-2xl mx-auto mt-4">
                                <h3 className="text-lg font-semibold mb-3">Search Results ({searchResults.length})</h3>
                                <div className="space-y-3 max-h-96 overflow-y-auto">
                                    {searchResults.map((result, index) => <ResourceCard key={index} item={result} />)}
                                    {searchResults.length === 0 && (
                                        <div className="text-center py-8 text-gray-500">No results found for "{searchQuery}"</div>
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
                            <button onClick={() => setCurrentPage('interests')} className="bg-white text-blue-600 px-4 py-2 rounded-lg font-semibold hover:bg-gray-100">
                                Get Started →
                            </button>
                        </div>
                        
                        <div className="bg-gradient-to-br from-green-500 to-teal-600 text-white p-6 rounded-lg shadow-lg">
                            <div className="flex items-center mb-4">
                                <span className="text-3xl mr-3">👥</span>
                                <h2 className="text-2xl font-bold">Student Stories</h2>
                            </div>
                            <p className="mb-4">Real stories and illustrative examples of student success</p>
                            <button onClick={() => setCurrentPage('stories')} className="bg-white text-green-600 px-4 py-2 rounded-lg font-semibold hover:bg-gray-100">
                                View Stories →
                            </button>
                        </div>
                    </div>
                    
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
                <div className="max-w-4xl mx-auto p-6">
                    <Breadcrumb />
                    <h1 className="text-3xl font-bold text-gray-800 mb-6">Choose Your Interest Area</h1>
                    
                    <div className="grid md:grid-cols-2 lg:grid-cols-3 gap-4">
                        {interests.map((interest) => (
                            <button
                                key={interest.id}
                                onClick={() => { setSelectedInterest(interest.id); setCurrentPage('details'); }}
                                className="bg-white p-6 rounded-lg shadow-md hover:shadow-lg transition-shadow border-2 border-transparent hover:border-blue-500 text-left"
                            >
                                <div className="flex items-center mb-3">
                                    <span className="text-2xl mr-3">{interest.icon}</span>
                                    <h3 className="text-lg font-semibold text-gray-800">{interest.name}</h3>
                                </div>
                                <span className="text-gray-400">→</span>
                            </button>
                        ))}
                    </div>
                </div>
            );

            const renderDetails = () => {
                const selectedData = data[selectedInterest];
                const selectedInterestData = interests.find(i => i.id === selectedInterest);

                return (
                    <div className="max-w-6xl mx-auto p-6">
                        <Breadcrumb />
                        
                        <div className="flex items-center mb-6">
                            <span className="text-3xl mr-3">{selectedInterestData?.icon}</span>
                            <h1 className="text-3xl font-bold text-gray-800">{selectedInterestData?.name}</h1>
                        </div>

                        {selectedInterest === 'general' ? (
                            <div>
                                <h2 className="text-2xl font-bold text-gray-800 mb-4">Supporting Resources</h2>
                                <div className="space-y-4">
                                    {selectedData.resources.map((resource, index) => <ResourceCard key={index} item={resource} />)}
                                </div>
                            </div>
                        ) : (
                            <div className="grid lg:grid-cols-2 gap-8">
                                <div>
                                    <h2 className="text-2xl font-bold text-gray-800 mb-4">Areas of Academic Focus</h2>
                                    <div className="space-y-4">
                                        {selectedData.majors.map((major, index) => <ResourceCard key={index} item={major} />)}
                                    </div>
                                </div>
                                
                                <div>
                                    <h2 className="text-2xl font-bold text-gray-800 mb-4">Supporting Resources</h2>
                                    <div className="space-y-4">
                                        {selectedData.resources.map((resource, index) => <ResourceCard key={index} item={resource} />)}
                                    </div>
                                </div>
                            </div>
                        )}
                        
                        <div className="mt-8 bg-blue-50 p-6 rounded-lg">
                            <h3 className="text-xl font-bold text-blue-800 mb-2">Next Steps</h3>
                            <p className="text-blue-700 mb-4">Ready to explore more? Check out our student stories to see how students combined different majors and resources.</p>
                            <button onClick={() => setCurrentPage('stories')} className="bg-blue-600 text-white px-4 py-2 rounded-lg font-semibold hover:bg-blue-700">
                                View Student Stories
                            </button>
                        </div>
                    </div>
                );
            };

            const renderStories = () => (
                <div className="max-w-6xl mx-auto p-6">
                    <Breadcrumb />
                    <h1 className="text-3xl font-bold text-gray-800 mb-6">Student Stories</h1>

                    <div className="mb-8">
                        <div className="flex items-center gap-4 mb-4">
                            <h2 className="text-2xl font-bold text-gray-800">Real Student Success Stories</h2>
                            <span className="bg-green-100 text-green-800 px-3 py-1 rounded-full text-sm font-medium">Verified Stories</span>
                        </div>
                        <div className="grid lg:grid-cols-2 gap-6">
                            {studentStories.filter(s => s.isReal).map((story, index) => <StoryCard key={index} story={story} />)}
                        </div>
                    </div>

                    <div className="mb-8">
                        <div className="flex items-center gap-4 mb-4">
                            <h2 className="text-2xl font-bold text-gray-800">Illustrative Student Journeys</h2>
                            <span className="bg-blue-100 text-blue-800 px-3 py-1 rounded-full text-sm font-medium">Example Paths</span>
                        </div>
                        <div className="mb-6 p-4 bg-blue-50 rounded-lg border border-blue-200">
                            <p className="text-sm text-blue-700">
                                <strong>The following scenarios represent illustrative journeys of students exploring different majors and the campus resources that help them reach their goals. While these examples are fictional, they are designed to reflect common experiences and challenges students face.</strong>
                            </p>
                        </div>
                        <div className="grid lg:grid-cols-2 gap-6">
                            {studentStories.filter(s => !s.isReal).map((story, index) => <StoryCard key={index} story={story} />)}
                        </div>
                    </div>
                    
                    <div className="mt-8 bg-green-50 p-6 rounded-lg text-center">
                        <h3 className="text-xl font-bold text-green-800 mb-2">Ready to Start Your Journey?</h3>
                        <p className="text-green-700 mb-4">Explore different interest areas to find the perfect combination of majors and resources for your goals.</p>
                        <button onClick={() => setCurrentPage('interests')} className="bg-green-600 text-white px-6 py-3 rounded-lg font-semibold hover:bg-green-700">
                            Explore by Interest
                        </button>
                    </div>
                </div>
            );

            return (
                <div className="min-h-screen bg-gradient-to-br from-blue-50 to-indigo-100">
                    <Navigation />
                    <main className="pb-20">
                        {currentPage === 'home' && renderHome()}
                        {currentPage === 'interests' && renderInterests()}
                        {currentPage === 'details' && renderDetails()}
                        {currentPage === 'stories' && renderStories()}
                    </main>
                </div>
            );
        };

        setTimeout(() => {
            ReactDOM.render(React.createElement(LifeSciencesExplorer), document.getElementById('root'));
        }, 1000);
    </script>
</body>
</html>
