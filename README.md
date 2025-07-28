<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>UConn Life Sciences Explorer</title>
    <script crossorigin src="https://unpkg.com/react@18/umd/react.development.js"></script>
    <script crossorigin src="https://unpkg.com/react-dom@18/umd/react-dom.development.js"></script>
    <script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        .loading { animation: spin 1s linear infinite; }
        @keyframes spin { from { transform: rotate(0deg); } to { transform: rotate(360deg); } }
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
            const [showMobileMenu, setShowMobileMenu] = useState(false);
            const [showScrollTop, setShowScrollTop] = useState(false);

            const interests = [
                { id: 'molecular-bio', name: 'Molecular Biology & Genetics', icon: '🧬' },
                { id: 'biotech', name: 'Biotechnology, Engineering & Innovation', icon: '🔬' },
                { id: 'healthcare', name: 'Healthcare & Medicine', icon: '❤️' },
                { id: 'research', name: 'Research & Academia', icon: '🔬' },
                { id: 'entrepreneurship', name: 'Biotech Entrepreneurship', icon: '💡' },
                { id: 'general', name: 'General Resources', icon: '👥' }
            ];

            const majors = {
                'molecular-bio': [
                    { name: 'Molecular & Cell Biology', description: 'Study life at the cellular and molecular level', url: 'https://mcb.uconn.edu/', type: 'major', department: 'CLAS' },
                    { name: 'Masters in Genetic Counseling', description: 'Explore heredity and genetic variation', url: 'https://geneticcounseling.uconn.edu/', type: 'graduate', department: 'Health' },
                    { name: 'Applied Biochemistry and Cell Biology', description: 'Masters Degree Program', url: 'https://mcb.uconn.edu/applied-biochemistry-and-cell-biology-overview/', type: 'graduate', department: 'CLAS' },
                    { name: 'Pathobiology and Veterinary Science', description: 'Study of disease processes and mechanisms', url: 'https://pathobiology.cahnr.uconn.edu/', type: 'major', department: 'CAHNR' },
                    { name: 'Diagnostic Genetic Sciences', description: 'Genetic testing and counseling sciences', url: 'https://alliedhealth.uconn.edu/dgs/', type: 'major', department: 'Health' }
                ],
                'biotech': [
                    { name: 'Biomedical Engineering and Innovation', description: 'Biomedical engineering with innovation focus', url: 'https://bioinnovation.engineering.uconn.edu/?utm', type: 'major', department: 'Engineering' },
                    { name: 'Chemical and Biomolecular Engineering', description: 'Design processes for biological materials', url: 'https://chemical-biomolecular.engineering.uconn.edu/', type: 'major', department: 'Engineering' },
                    { name: 'Biomaterials and Tissue Engineering', description: 'Develop biomaterials and medical devices', url: 'https://mse.engr.uconn.edu/biomaterials-and-tissue-engineering', type: 'specialization', department: 'Engineering' },
                    { name: 'Innovation & Entrepreneurship', description: 'College of Engineering & ehub', url: 'https://engineering.uconn.edu/research/innovation-entrepreneurship/', type: 'program', department: 'Engineering' },
                    { name: 'Agricultural and Health Biotechnology', description: 'Agricultural biotechnology and health applications', url: 'https://www.nifa.usda.gov/about-nifa/blogs/exploring-ag-biotech-careers-university-connecticut-4-h', type: 'program', department: 'CAHNR' }
                ],
                'healthcare': [
                    { name: 'Nursing', description: 'Direct patient care and health promotion', url: 'https://nursing.uconn.edu/', type: 'major', department: 'Health' },
                    { name: 'Pre-Medicine', description: 'Prepare for medical school', url: 'https://premed.uconn.edu/', type: 'track', department: 'Health' },
                    { name: 'Master in Public Health', description: 'Population health and disease prevention', url: 'https://mph.uconn.edu/', type: 'graduate', department: 'Health' },
                    { name: 'Nutritional Sciences', description: 'Study nutrition and metabolism', url: 'https://nusc.uconn.edu/', type: 'major', department: 'CAHNR' },
                    { name: 'Pathobiology and Veterinary Science', description: 'Study of disease processes and mechanisms', url: 'https://pathobiology.cahnr.uconn.edu/', type: 'major', department: 'CAHNR' },
                    { name: 'Diagnostic Genetic Sciences', description: 'Genetic testing and counseling sciences', url: 'https://alliedhealth.uconn.edu/dgs/', type: 'major', department: 'Health' },
                    { name: 'Pharmacy Research', description: 'Center for Pharmaceutical Processing Research', url: 'https://pharmacy.uconn.edu/research/', type: 'research', department: 'Health' }
                ],
                'research': [
                    { name: 'Ecology and Evolutionary Biology', description: 'Broad study of living organisms', url: 'https://eeb.uconn.edu/', type: 'major', department: 'CLAS' },
                    { name: 'Bioinformatics', description: 'Computational analysis of biological data', url: 'https://computing.engineering.uconn.edu/research/areas/bio-and-medical-informatics/', type: 'specialization', department: 'Engineering' },
                    { name: 'Statistics', description: 'Data analysis for biological research', url: 'https://stat.uconn.edu/', type: 'major', department: 'CLAS' }
                ],
                'entrepreneurship': [
                    { name: 'Biology Majors & Minors', description: 'Combine business skills with scientific knowledge', url: 'https://biology.clas.uconn.edu/undergraduate/', type: 'major', department: 'CLAS' },
                    { name: 'Engineering + Innovation Minor', description: 'Technical skills with entrepreneurial mindset', url: 'https://engineering.uconn.edu/', type: 'minor', department: 'Engineering' },
                    { name: 'Biology + Computer Science', description: 'Bioinformatics and computational biology', url: 'https://computing.engineering.uconn.edu/', type: 'double-major', department: 'Multiple' }
                ]
            };

            const resources = {
                'molecular-bio': [
                    { name: 'Institute for Systems Genomics (ISG)', url: 'https://isg.uconn.edu/undergraduates/', description: 'Undergraduate research opportunities in genomics', type: 'research', department: 'Research' },
                    { name: 'Center for Genome Innovation', url: 'https://genome.center.uconn.edu/', description: 'Cutting-edge genomics research facility', type: 'facility', department: 'Research' },
                    { name: 'Research Resources & Equipment', url: 'https://core.uconn.edu/campus/storrs/', description: 'Access to specialized research facilities', type: 'facility', department: 'Research' },
                    { name: 'UConn iGEM Club', url: 'https://uconntact.uconn.edu/organization/uconnget', description: 'Synthetic biology competition team', type: 'club', department: 'Student Life' },
                    { name: 'UConn Biotech Club', url: 'https://uconntact.uconn.edu/organization/uconnbiotechclub', description: 'Student organization for biotech enthusiasts', type: 'club', department: 'Student Life' },
                    { name: 'TIP Innovation Fellowship', url: 'https://innovation.uconn.edu/technology-incubation-program/summer-fellowship/', description: 'Summer research and innovation program', type: 'program', department: 'Innovation' }
                ],
                'biotech': [
                    { name: 'Center for Open Research Resources & Equipment (COR-E)', url: 'https://core.uconn.edu/storrs-facilities/', description: 'Access to advanced research equipment', type: 'facility', department: 'Research' },
                    { name: '4-H Biotechnology Career Readiness Program', url: 'https://publications.extension.uconn.edu/tag/biotechnology-club/', description: 'Exploring Ag Biotech Careers with UConn 4-H', type: 'program', department: 'CAHNR' },
                    { name: 'UConn Biotech Club', url: 'https://uconntact.uconn.edu/organization/uconnbiotechclub', description: 'Active student biotechnology organization', type: 'club', department: 'Student Life' },
                    { name: 'Connecticut Center for Entrepreneurship and Innovation', url: 'https://ccei.uconn.edu/', description: 'Entrepreneurship education and support', type: 'center', department: 'Business' },
                    { name: 'Innovation Shop', url: 'https://innovationshop.engineering.uconn.edu/machine-shop/', description: 'College of engineering', type: 'facility', department: 'Engineering' },
                    { name: 'Innovation Labs', url: 'https://innovatelabs.business.uconn.edu/', description: 'School of business', type: 'facility', department: 'Business' },
                    { name: 'TIP Innovation Fellowship', url: 'https://innovation.uconn.edu/technology-incubation-program/summer-fellowship/', description: 'Summer research and innovation program', type: 'program', department: 'Innovation' },
                    { name: 'iGEM Club', url: 'https://uconntact.uconn.edu/organization/uconnget', description: 'International Genetically Engineered Machine competition team', type: 'club', department: 'Student Life' }
                ],
                'healthcare': [
                    { name: 'Nursing and Engineering Innovation Center', url: 'https://nursing-engineering-innovation.center.uconn.edu/', description: 'Interdisciplinary healthcare innovation', type: 'center', department: 'Health' },
                    { name: 'Healthcare Innovation Certificate', url: 'https://nursing.uconn.edu/research-innovation/innovation-new-knowledge/', description: 'Specialized healthcare innovation training', type: 'certificate', department: 'Health' },
                    { name: 'UConn Health Center Farmington', description: 'Clinical experience and research opportunities', url: 'https://ugradresearch.uconn.edu/hrp/', type: 'facility', department: 'Health' },
                    { name: 'UConn Biotech Club', url: 'https://uconntact.uconn.edu/organization/uconnbiotechclub', description: 'Student organization for biotech enthusiasts', type: 'club', department: 'Student Life' },
                    { name: 'UConn AAPS Student Chapter', url: 'https://pharmacy.uconn.edu/aaps-student-chapter/', description: 'Student chapter for pharmaceutical sciences', type: 'club', department: 'Health' },
                    { name: 'TIP Innovation Fellowship', url: 'https://innovation.uconn.edu/technology-incubation-program/summer-fellowship/', description: 'Summer research and innovation program', type: 'program', department: 'Innovation' }
                ],
                'research': [
                    { name: 'Institute for Systems Genomics (ISG)', url: 'https://isg.uconn.edu/undergraduates/', description: 'Research opportunities in systems biology', type: 'research', department: 'Research' },
                    { name: 'Jackson Labs Genomics Course', url: 'https://www.jax.org/education-and-learning/high-school-students-and-undergraduates/', description: 'Renowned genetics research institution', type: 'program', department: 'External' },
                    { name: 'UConn Library Maker Studio', url: 'https://lib.uconn.edu/location/homer-babbidge-library/maker-studio-uconn-library/', description: 'Digital fabrication and prototyping', type: 'facility', department: 'Library' },
                    { name: 'UConn Biotech Club', url: 'https://uconntact.uconn.edu/organization/uconnbiotechclub', description: 'Student organization for biotech enthusiasts', type: 'club', department: 'Student Life' },
                    { name: 'TIP Innovation Fellowship', url: 'https://innovation.uconn.edu/technology-incubation-program/summer-fellowship/', description: 'Summer research and innovation program', type: 'program', department: 'Innovation' },
                    { name: 'Connecticut Center for Entrepreneurship and Innovation', url: 'https://ccei.uconn.edu/', description: 'Entrepreneurship education and support', type: 'center', department: 'Business' },
                    { name: 'iGEM Club', url: 'https://uconntact.uconn.edu/organization/uconnget', description: 'International Genetically Engineered Machine competition team', type: 'club', department: 'Student Life' }
                ],
                'entrepreneurship': [
                    { name: 'Connecticut Center for Entrepreneurship and Innovation (CCEI)', url: 'https://ccei.uconn.edu/', description: 'Entrepreneurship education and support', type: 'center', department: 'Business' },
                    { name: 'Peter J. Werth Institute for Entrepreneurship & Innovation', url: 'https://werth.institute.uconn.edu/', description: 'Innovation and startup incubation', type: 'center', department: 'Business' },
                    { name: 'Daigle Labs', url: 'https://daiglelabs.business.uconn.edu/', description: 'Student-led business incubator', type: 'incubator', department: 'Business' },
                    { name: 'Innovation House Learning Community', url: 'https://lc.uconn.edu/innovationhouse/', description: 'Residential program for innovators', type: 'program', department: 'Student Life' },
                    { name: 'TIP Innovation Fellowship', url: 'https://innovation.uconn.edu/technology-incubation-program/summer-fellowship/', description: 'Summer research and innovation program', type: 'program', department: 'Innovation' },
                    { name: 'Hillside Ventures', url: 'https://hillsideventures.uconn.edu/', description: 'Student-led venture capital program', type: 'program', department: 'Business' },
                    { name: 'Get Seeded Nights', url: 'https://ccei.uconn.edu/getseeded/', description: 'Pitch competitions and networking events', type: 'event', department: 'Business' },
                    { name: 'UConn Biotech Club', url: 'https://uconntact.uconn.edu/organization/uconnbiotechclub', description: 'Student organization for biotech enthusiasts', type: 'club', department: 'Student Life' }
                ],
                'general': [
                    { name: 'UConn Overview', url: 'https://uconn.edu/', description: 'Programs spanning multiple departments', type: 'general', department: 'University' },
                    { name: 'NetWerx Alumni Mentorship Program', url: 'https://today.uconn.edu/2025/01/new-netwerx-initiative-brings-alumni-mentorship-into-the-classroom/', description: 'Connect with alumni mentors to build networking and entrepreneurial skills', type: 'program', department: 'Alumni' },
                    { name: 'UConn Tech Park', url: 'https://techpark.uconn.edu/', description: 'Hub for cutting-edge research, industry collaboration, and innovation', type: 'facility', department: 'Research' },
                    { name: 'Technology Commercialization Internship', url: 'https://innovation.uconn.edu/incubator/tcsinternship/', description: 'TCS goal is to facilitate the transition of UConn discoveries into products and services that benefit patients, industry and society.', type: 'internship', department: 'Innovation' },
                    { name: 'Accelerate UConn (NSF I-Corps)', url: 'https://ccei.uconn.edu/programs/accelerate-uconn/', description: 'National Science Foundation I-Corps Program', type: 'program', department: 'Business' }
                ]
            };

            const realStudentStories = [
                {
                    title: "Maya Patel - Diagnostic Genetic Sciences",
                    major: "Diagnostic Genetic Sciences",
                    minor: "Research",
                    story: "Maya shares: 'Being part of DGS opened my eyes to how genetics impacts healthcare.' Her specialized program led to meaningful career preparation.",
                    resources: ["Institute for Systems Genomics", "DGS Program"],
                    outcome: "Preparing for genetic counseling career",
                    url: "https://genomics.institute.uconn.edu/",
                    isReal: true
                },
                {
                    title: "Tyler & Lyla - Pharmacy Research",
                    major: "Pharmacy",
                    minor: "Research",
                    story: "Two pharmacy students received Gateway to Research Awards. Tyler studies fentanyl treatments while Lyla develops aspirin formulations.",
                    resources: ["AFPE Award", "Pharmacy Research"],
                    outcome: "Advancing pharmaceutical science",
                    url: "https://today.uconn.edu/2024/05/two-school-of-pharmacy-students/",
                    isReal: true
                },
                {
                    title: "Reza Amin - Healthcare Entrepreneur",
                    major: "Engineering PhD",
                    minor: "Entrepreneurship MS",
                    story: "Founded Bastion Health, largest virtual urology group serving 120,000 men nationwide with AI-powered diagnostics.",
                    resources: ["TIP", "Werth Institute", "CCEI"],
                    outcome: "Multi-million dollar healthcare company CEO",
                    url: "https://today.uconn.edu/2025/06/uconn-entrepreneur/",
                    isReal: true
                },
                {
                    title: "Pathobiology Success Story",
                    major: "Pathobiology and Veterinary Science",
                    minor: "Research",
                    story: "Alumni from the Pathobiology program have gone on to successful careers in veterinary medicine, research, and public health, demonstrating the diverse opportunities available in this field.",
                    resources: ["Pathobiology Program", "Research Opportunities", "UConn Health"],
                    outcome: "Careers in veterinary medicine and disease research",
                    url: "https://pathobiology.cahnr.uconn.edu/alumni/",
                    isReal: true
                }
            ];

            const illustrativeJourneys = [
                {
                    title: "Alex - Healthcare Innovation",
                    major: "Biomedical Engineering",
                    minor: "Entrepreneurship",
                    story: "Combined engineering with business skills, used Innovation Shop for prototypes.",
                    resources: ["Innovation Shop", "CCEI"],
                    outcome: "AI diagnostic tools developer",
                    isReal: false
                },
                {
                    title: "Sam's Route: Research Scientist",
                    major: "Ecology and Evolutionary Biology",
                    minor: "Statistics",
                    story: "Sam wanted to conduct cutting-edge research. They started in the ISG as a freshman, learned data analysis skills, and spent summers at Jackson Labs doing genetics research. The Innovation Fellowship Program provided additional research opportunities.",
                    resources: ["Institute for Systems Genomics", "Jackson Labs Genomics Course", "Innovation Fellowship Program", "Center for Genome Innovation"],
                    outcome: "PhD candidate studying cancer genomics with multiple publications",
                    isReal: false
                },
                {
                    title: "Jordan's Track: Clinical Practice",
                    major: "Nursing",
                    minor: "Innovation & Entrepreneurship",  
                    story: "Jordan chose nursing for direct patient care but wanted to improve healthcare systems. They joined the Innovation House, participated in the Innovation Fellowship Program, and worked on healthcare technology projects with support from the Connecticut Center for Entrepreneurship and Innovation.",
                    resources: ["Innovation House", "Innovation Fellowship Program", "Connecticut Center for Entrepreneurship and Innovation", "UConn Health Center Farmington"],
                    outcome: "Nurse practitioner leading digital health initiatives in hospitals",
                    isReal: false
                }
            ];

            useEffect(() => {
                const handleScroll = () => {
                    setShowScrollTop(window.scrollY > 300);
                };
                window.addEventListener('scroll', handleScroll);
                return () => window.removeEventListener('scroll', handleScroll);
            }, []);

            const scrollToTop = () => {
                window.scrollTo({ top: 0, behavior: 'smooth' });
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
                                resultType: 'major',
                                category: interests.find(i => i.id === interestId)?.name || 'General',
                                interestId
                            });
                        }
                    });
                });

                Object.entries(resources).forEach(([interestId, resourcesList]) => {
                    resourcesList.forEach(resource => {
                        if (resource.name.toLowerCase().includes(searchTerm) || resource.description.toLowerCase().includes(searchTerm)) {
                            results.push({
                                ...resource,
                                resultType: 'resource',
                                category: interests.find(i => i.id === interestId)?.name || 'General',
                                interestId
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

            const Breadcrumb = () => {
                const pages = [];
                
                if (currentPage === 'home') {
                    pages.push({ name: 'Home', active: true });
                } else if (currentPage === 'interests') {
                    pages.push({ name: 'Home', onClick: () => setCurrentPage('home') });
                    pages.push({ name: 'Interests', active: true });
                } else if (currentPage === 'details') {
                    pages.push({ name: 'Home', onClick: () => setCurrentPage('home') });
                    pages.push({ name: 'Interests', onClick: () => setCurrentPage('interests') });
                    pages.push({ name: selectedInterest ? interests.find(i => i.id === selectedInterest)?.name : 'Details', active: true });
                } else if (currentPage === 'case-studies') {
                    pages.push({ name: 'Home', onClick: () => setCurrentPage('home') });
                    pages.push({ name: 'Student Stories', active: true });
                }

                return (
                    <nav className="flex mb-4" aria-label="Breadcrumb">
                        <ol className="inline-flex items-center space-x-1 md:space-x-3">
                            {pages.map((page, index) => (
                                <li key={index} className="inline-flex items-center">
                                    {index > 0 && (
                                        <span className="w-4 h-4 text-gray-400 mx-1">→</span>
                                    )}
                                    {page.active ? (
                                        <span className="text-gray-500 text-sm font-medium">{page.name}</span>
                                    ) : (
                                        <button
                                            onClick={page.onClick}
                                            className="text-blue-600 text-sm font-medium hover:text-blue-800 focus:outline-none focus:ring-2 focus:ring-blue-500 rounded"
                                        >
                                            {page.name}
                                        </button>
                                    )}
                                </li>
                            ))}
                        </ol>
                    </nav>
                );
            };

            const SearchBar = ({ className = "" }) => (
                <div className={`relative ${className}`}>
                    <span className="absolute left-3 top-1/2 transform -translate-y-1/2 text-gray-400">🔍</span>
                    <input
                        type="text"
                        placeholder="Search majors, resources, or student journeys..."
                        value={searchQuery}
                        onChange={(e) => setSearchQuery(e.target.value)}
                        className="w-full pl-10 pr-4 py-3 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-500"
                    />
                    {searchQuery && (
                        <button
                            onClick={() => setSearchQuery('')}
                            className="absolute right-3 top-1/2 transform -translate-y-1/2 text-gray-400 hover:text-gray-600"
                            aria-label="Clear search"
                        >
                            ✕
                        </button>
                    )}
                </div>
            );

            const ResourceCard = ({ resource }) => (
                <div className="bg-white p-4 rounded-lg shadow-md border-l-4 border-blue-500 hover:shadow-lg transition-shadow">
                    <div className="flex items-start justify-between">
                        <div className="flex-1 pr-4">
                            <div className="flex items-center gap-2 mb-2">
                                <h4 className="font-semibold text-gray-800">{resource.name}</h4>
                            </div>
                            <p className="text-sm text-gray-600 mb-2">{resource.description}</p>
                            <div className="flex flex-wrap gap-1 mb-2">
                                {resource.type && (
                                    <span className="text-xs bg-blue-100 text-blue-600 px-2 py-1 rounded">
                                        {resource.type}
                                    </span>
                                )}
                                {resource.department && (
                                    <span className="text-xs bg-gray-100 text-gray-600 px-2 py-1 rounded">
                                        {resource.department}
                                    </span>
                                )}
                            </div>
                        </div>
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
                    <div className="bg-green-50 p-3
