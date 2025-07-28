<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>UConn Life Sciences Explorer</title>
    <style>
        * { margin: 0; padding: 0; box-sizing: border-box; }
        body { 
            font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            min-height: 100vh;
            color: #333;
        }
        .container { max-width: 1200px; margin: 0 auto; padding: 20px; }
        .header { text-align: center; color: white; margin-bottom: 40px; }
        .nav { background: white; border-radius: 12px; padding: 20px; margin-bottom: 30px; box-shadow: 0 4px 20px rgba(0,0,0,0.1); }
        .nav-title { font-size: 24px; font-weight: bold; color: #3b82f6; margin-bottom: 20px; }
        .search-box { 
            width: 100%; 
            padding: 15px 20px; 
            border: 2px solid #e5e7eb; 
            border-radius: 8px; 
            font-size: 16px;
            outline: none;
            transition: border-color 0.3s;
        }
        .search-box:focus { border-color: #3b82f6; }
        .card { 
            background: white; 
            padding: 25px; 
            margin: 20px 0; 
            border-radius: 12px; 
            box-shadow: 0 4px 15px rgba(0,0,0,0.1);
            border-left: 5px solid #3b82f6;
        }
        .card h2 { color: #1f2937; margin-bottom: 15px; font-size: 24px; }
        .card h3 { color: #374151; margin: 15px 0 8px 0; font-size: 18px; }
        .card p { color: #6b7280; margin-bottom: 10px; line-height: 1.6; }
        .link { 
            color: #3b82f6; 
            text-decoration: none; 
            font-weight: 600;
            border: 2px solid #3b82f6;
            padding: 8px 16px;
            border-radius: 6px;
            display: inline-block;
            margin: 5px 10px 5px 0;
            transition: all 0.3s;
        }
        .link:hover { 
            background: #3b82f6; 
            color: white;
            transform: translateY(-2px);
        }
        .disclaimer { 
            background: #fef3c7; 
            border-left: 5px solid #f59e0b; 
            padding: 20px; 
            border-radius: 8px; 
            margin: 25px 0;
        }
        .disclaimer h3 { color: #92400e; margin-bottom: 10px; }
        .disclaimer p { color: #92400e; font-size: 14px; }
        .grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(300px, 1fr)); gap: 20px; }
        .feature { text-align: center; padding: 20px; }
        .feature-icon { font-size: 48px; margin-bottom: 15px; }
        .btn { 
            background: #3b82f6; 
            color: white; 
            padding: 15px 30px; 
            border: none; 
            border-radius: 8px; 
            font-size: 16px; 
            font-weight: 600;
            cursor: pointer;
            transition: all 0.3s;
            text-decoration: none;
            display: inline-block;
        }
        .btn:hover { 
            background: #2563eb; 
            transform: translateY(-2px);
        }
        .search-results { 
            background: white; 
            border-radius: 8px; 
            padding: 20px; 
            margin-top: 20px;
            max-height: 400px;
            overflow-y: auto;
        }
        .result-item { 
            padding: 15px; 
            border-bottom: 1px solid #e5e7eb; 
            border-left: 3px solid #3b82f6;
            margin-bottom: 10px;
            background: #f9fafb;
            border-radius: 6px;
        }
        .result-item:last-child { border-bottom: none; }
        .result-title { font-weight: 600; color: #1f2937; margin-bottom: 5px; }
        .result-desc { color: #6b7280; font-size: 14px; margin-bottom: 8px; }
        .result-link { 
            color: #3b82f6; 
            text-decoration: none; 
            font-size: 14px; 
            font-weight: 600;
        }
        @media (max-width: 768px) {
            .container { padding: 10px; }
            .nav { padding: 15px; }
            .card { padding: 20px; }
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="header">
            <h1 style="font-size: 2.5rem; margin-bottom: 10px;">🧬 UConn Life Sciences & Biotech Explorer</h1>
            <p style="font-size: 1.2rem; opacity: 0.9;">Your comprehensive guide to life sciences programs at UConn</p>
            <p style="font-size: 0.9rem; opacity: 0.7;">Last Updated: July 2025</p>
        </div>
        
        <div class="nav">
            <div class="nav-title">🔍 Explore Programs & Resources</div>
            <input 
                type="text" 
                class="search-box" 
                placeholder="Search majors, resources, or opportunities..." 
                id="searchInput"
                oninput="performSearch()"
            >
            <div id="searchResults"></div>
        </div>

        <div class="disclaimer">
            <h3>⚠️ Important Disclaimer</h3>
            <p>This application is an independent project and is not affiliated with or endorsed by the University of Connecticut. The information provided is for general guidance only and may not reflect the most current university policies, programs, or resources. Users should verify details with official UConn sources.</p>
        </div>

        <div class="card">
            <h2>🧬 Molecular Biology & Genetics</h2>
            <h3>Areas of Academic Focus:</h3>
            <p><strong>Molecular & Cell Biology:</strong> Study life at the cellular and molecular level</p>
            <a href="https://mcb.uconn.edu/" target="_blank" class="link">Learn More</a>
            
            <p><strong>Masters in Genetic Counseling:</strong> Explore heredity and genetic variation</p>
            <a href="https://geneticcounseling.uconn.edu/" target="_blank" class="link">Learn More</a>
            
            <p><strong>Diagnostic Genetic Sciences:</strong> Genetic testing and counseling sciences</p>
            <a href="https://alliedhealth.uconn.edu/dgs/" target="_blank" class="link">Learn More</a>
            
            <h3>Supporting Resources:</h3>
            <p><strong>Institute for Systems Genomics (ISG):</strong> Undergraduate research opportunities</p>
            <a href="https://isg.uconn.edu/undergraduates/" target="_blank" class="link">Visit ISG</a>
            
            <p><strong>UConn Biotech Club:</strong> Student organization for biotech enthusiasts</p>
            <a href="https://uconntact.uconn.edu/organization/uconnbiotechclub" target="_blank" class="link">Join Club</a>
        </div>

        <div class="card">
            <h2>🔬 Biotechnology, Engineering & Innovation</h2>
            <h3>Areas of Academic Focus:</h3>
            <p><strong>Biomedical Engineering and Innovation:</strong> Biomedical engineering with innovation focus</p>
            <a href="https://bioinnovation.engineering.uconn.edu/?utm" target="_blank" class="link">Learn More</a>
            
            <p><strong>Chemical and Biomolecular Engineering:</strong> Design processes for biological materials</p>
            <a href="https://chemical-biomolecular.engineering.uconn.edu/" target="_blank" class="link">Learn More</a>
            
            <p><strong>Agricultural and Health Biotechnology:</strong> Agricultural biotech and health applications</p>
            <a href="https://www.nifa.usda.gov/about-nifa/blogs/exploring-ag-biotech-careers-university-connecticut-4-h" target="_blank" class="link">Learn More</a>
            
            <h3>Supporting Resources:</h3>
            <p><strong>Innovation Shop:</strong> College of engineering maker space</p>
            <a href="https://innovationshop.engineering.uconn.edu/machine-shop/" target="_blank" class="link">Visit Shop</a>
            
            <p><strong>Innovation Labs:</strong> School of business innovation space</p>
            <a href="https://innovatelabs.business.uconn.edu/" target="_blank" class="link">Visit Labs</a>
            
            <p><strong>Connecticut Center for Entrepreneurship and Innovation:</strong> Startup support</p>
            <a href="https://ccei.uconn.edu/" target="_blank" class="link">Visit CCEI</a>
        </div>

        <div class="card">
            <h2>❤️ Healthcare & Medicine</h2>
            <h3>Areas of Academic Focus:</h3>
            <p><strong>Nursing:</strong> Direct patient care and health promotion</p>
            <a href="https://nursing.uconn.edu/" target="_blank" class="link">Learn More</a>
            
            <p><strong>Pre-Medicine:</strong> Prepare for medical school</p>
            <a href="https://premed.uconn.edu/" target="_blank" class="link">Learn More</a>
            
            <p><strong>Master in Public Health:</strong> Population health and disease prevention</p>
            <a href="https://mph.uconn.edu/" target="_blank" class="link">Learn More</a>
            
            <h3>Supporting Resources:</h3>
            <p><strong>UConn Health Center Farmington:</strong> Clinical experience opportunities</p>
            <a href="https://ugradresearch.uconn.edu/hrp/" target="_blank" class="link">Learn More</a>
        </div>

        <div class="card">
            <h2>🔬 Research & Academia</h2>
            <h3>Areas of Academic Focus:</h3>
            <p><strong>Ecology and Evolutionary Biology:</strong> Broad study of living organisms</p>
            <a href="https://eeb.uconn.edu/" target="_blank" class="link">Learn More</a>
            
            <p><strong>Bioinformatics:</strong> Computational analysis of biological data</p>
            <a href="https://computing.engineering.uconn.edu/research/areas/bio-and-medical-informatics/" target="_blank" class="link">Learn More</a>
            
            <h3>Supporting Resources:</h3>
            <p><strong>Institute for Systems Genomics:</strong> Research opportunities in systems biology</p>
            <a href="https://isg.uconn.edu/undergraduates/" target="_blank" class="link">Learn More</a>
        </div>

        <div class="card">
            <h2>💡 Biotech Entrepreneurship</h2>
            <h3>Areas of Academic Focus:</h3>
            <p><strong>Biology + Business Skills:</strong> Combine scientific knowledge with entrepreneurship</p>
            <a href="https://biology.clas.uconn.edu/undergraduate/" target="_blank" class="link">Biology Programs</a>
            
            <p><strong>Engineering + Innovation Minor:</strong> Technical skills with entrepreneurial mindset</p>
            <a href="https://engineering.uconn.edu/" target="_blank" class="link">Learn More</a>
            
            <h3>Supporting Resources:</h3>
            <p><strong>Connecticut Center for Entrepreneurship and Innovation (CCEI):</strong> Comprehensive startup support</p>
            <a href="https://ccei.uconn.edu/" target="_blank" class="link">Visit CCEI</a>
            
            <p><strong>Peter J. Werth Institute:</strong> Innovation and startup incubation</p>
            <a href="https://werth.institute.uconn.edu/" target="_blank" class="link">Learn More</a>
        </div>

        <div class="card">
            <h2>🎯 General Resources</h2>
            <p><strong>UConn Tech Park:</strong> Hub for cutting-edge research and industry collaboration</p>
            <a href="https://techpark.uconn.edu/" target="_blank" class="link">Visit Tech Park</a>
            
            <p><strong>NetWerx Alumni Mentorship:</strong> Connect with alumni mentors for networking and career guidance</p>
            <a href="https://today.uconn.edu/2025/01/new-netwerx-initiative-brings-alumni-mentorship-into-the-classroom/" target="_blank" class="link">Learn More</a>
            
            <p><strong>Main UConn Website:</strong> Official university information and programs</p>
            <a href="https://uconn.edu/" target="_blank" class="link">Visit UConn</a>
        </div>

        <div class="card" style="background: linear-gradient(135deg, #667eea 0%, #764ba2 100%); color: white; text-align: center;">
            <h2 style="color: white;">🚀 Ready to Start Your Journey?</h2>
            <div class="grid">
                <div class="feature">
                    <div class="feature-icon">🔬</div>
                    <h3>World-Class Research</h3>
                    <p>Access cutting-edge facilities and renowned faculty</p>
                </div>
                <div class="feature">
                    <div class="feature-icon">💡</div>
                    <h3>Innovation Ecosystem</h3>
                    <p>Entrepreneur programs and startup support</p>
                </div>
                <div class="feature">
                    <div class="feature-icon">❤️</div>
                    <h3>Healthcare Connection</h3>
                    <p>Direct links to UConn Health and clinical experience</p>
                </div>
            </div>
            <a href="https://uconn.edu/" target="_blank" class="btn" style="margin-top: 20px;">Explore UConn Official Site</a>
        </div>
    </div>

    <script>
        const allContent = [
            { name: 'Molecular & Cell Biology', description: 'Study life at the cellular and molecular level', url: 'https://mcb.uconn.edu/', category: 'Molecular Biology' },
            { name: 'Genetic Counseling', description: 'Explore heredity and genetic variation', url: 'https://geneticcounseling.uconn.edu/', category: 'Molecular Biology' },
            { name: 'Diagnostic Genetic Sciences', description: 'Genetic testing and counseling sciences', url: 'https://alliedhealth.uconn.edu/dgs/', category: 'Molecular Biology' },
            { name: 'Biomedical Engineering and Innovation', description: 'Biomedical engineering with innovation focus', url: 'https://bioinnovation.engineering.uconn.edu/?utm', category: 'Biotechnology' },
            { name: 'Chemical and Biomolecular Engineering', description: 'Design processes for biological materials', url: 'https://chemical-biomolecular.engineering.uconn.edu/', category: 'Biotechnology' },
            { name: 'Innovation Shop', description: 'College of engineering maker space', url: 'https://innovationshop.engineering.uconn.edu/machine-shop/', category: 'Biotechnology' },
            { name: 'Innovation Labs', description: 'School of business innovation space', url: 'https://innovatelabs.business.uconn.edu/', category: 'Biotechnology' },
            { name: 'Nursing', description: 'Direct patient care and health promotion', url: 'https://nursing.uconn.edu/', category: 'Healthcare' },
            { name: 'Pre-Medicine', description: 'Prepare for medical school', url: 'https://premed.uconn.edu/', category: 'Healthcare' },
            { name: 'Public Health', description: 'Population health and disease prevention', url: 'https://mph.uconn.edu/', category: 'Healthcare' },
            { name: 'UConn Biotech Club', description: 'Student organization for biotech enthusiasts', url: 'https://uconntact.uconn.edu/organization/uconnbiotechclub', category: 'General' },
            { name: 'CCEI', description: 'Connecticut Center for Entrepreneurship and Innovation', url: 'https://ccei.uconn.edu/', category: 'Entrepreneurship' }
        ];

        function performSearch() {
            const query = document.getElementById('searchInput').value.toLowerCase();
            const resultsDiv = document.getElementById('searchResults');
            
            if (!query.trim()) {
                resultsDiv.innerHTML = '';
                return;
            }

            const results = allContent.filter(item => 
                item.name.toLowerCase().includes(query) || 
                item.description.toLowerCase().includes(query) ||
                item.category.toLowerCase().includes(query)
            );

            if (results.length === 0) {
                resultsDiv.innerHTML = '<div class="search-results"><p>No results found for "' + query + '"</p></div>';
                return;
            }

            let html = '<div class="search-results"><h3>Search Results (' + results.length + ')</h3>';
            results.forEach(item => {
                html += '<div class="result-item">';
                html += '<div class="result-title">' + item.name + '</div>';
                html += '<div class="result-desc">' + item.description + '</div>';
                html += '<span style="font-size: 12px; background: #e5e7eb; padding: 4px 8px; border-radius: 4px; color: #6b7280;">' + item.category + '</span> ';
                html += '<a href="' + item.url + '" target="_blank" class="result-link">Learn More →</a>';
                html += '</div>';
            });
            html += '</div>';
            
            resultsDiv.innerHTML = html;
        }
    </script>
</body>
</html>
