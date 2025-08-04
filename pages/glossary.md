---
title: Glossary
layout: page
permalink: /glossary.html
---
# Glossary

<div id="glossary-container"></div>

<div id="glossary-container"></div>

<script>
// Real-time Glossary Generator
class GlossaryGenerator {
    constructor(csvUrl, containerId = 'glossary-container') {
        this.csvUrl = csvUrl;
        this.containerId = containerId;
        this.glossaryData = [];
    }

    // Parse CSV text into array of objects
    parseCSV(csvText) {
        const lines = csvText.trim().split('\n');
        if (lines.length < 2) return [];

        // Get headers and clean them
        const headers = lines[0].split(',').map(header => 
            header.trim().replace(/"/g, '').toLowerCase().replace(/-/g, '_')
        );

        const data = [];
        
        // Process each data row
        for (let i = 1; i < lines.length; i++) {
            const values = this.parseCSVLine(lines[i]);
            if (values.length === 0) continue;

            const row = {};
            headers.forEach((header, index) => {
                row[header] = values[index] || '';
            });

            // Map to expected column names (flexible matching)
            const entry = this.mapToGlossaryEntry(row);
            // Only require Spanish word and English equivalent
            if (entry.spanish_word.trim() && entry.english_equivalent.trim()) {
                data.push(entry);
            }
        }

        return data;
    }

    // Parse a single CSV line handling quotes and commas
    parseCSVLine(line) {
        const result = [];
        let current = '';
        let inQuotes = false;
        
        for (let i = 0; i < line.length; i++) {
            const char = line[i];
            
            if (char === '"') {
                inQuotes = !inQuotes;
            } else if (char === ',' && !inQuotes) {
                result.push(current.trim());
                current = '';
            } else {
                current += char;
            }
        }
        
        result.push(current.trim());
        return result.map(field => field.replace(/^"|"$/g, ''));
    }

    // Map CSV row to glossary entry format
    mapToGlossaryEntry(row) {
        const entry = {
            spanish_word: '',
            english_equivalent: '',
            source: '',
            comment: ''
        };

        // Flexible column matching
        Object.keys(row).forEach(key => {
            const cleanKey = key.toLowerCase().trim();
            
            if (cleanKey.includes('spanish') && cleanKey.includes('word')) {
                entry.spanish_word = row[key];
            } else if (cleanKey.includes('english') && (cleanKey.includes('equivalent') || cleanKey.includes('translation'))) {
                entry.english_equivalent = row[key];
            } else if (cleanKey.includes('source')) {
                entry.source = row[key];
            } else if (cleanKey.includes('comment')) {
                entry.comment = row[key];
            }
        });

        return entry;
    }

    // Fetch CSV data from Google Docs
    async fetchCSVData() {
        try {
            const response = await fetch(this.csvUrl);
            if (!response.ok) {
                throw new Error(`HTTP error! status: ${response.status}`);
            }
            return await response.text();
        } catch (error) {
            console.error('Error fetching CSV data:', error);
            throw error;
        }
    }

    // Create loading spinner
    createLoadingElement() {
        return `
            <div class="glossary-loading" style="text-align: center; padding: 20px;">
                <div style="display: inline-block; width: 20px; height: 20px; border: 3px solid #f3f3f3; border-top: 3px solid #3498db; border-radius: 50%; animation: spin 1s linear infinite;"></div>
                <p style="margin-top: 10px;">Loading glossary...</p>
            </div>
            <style>
                @keyframes spin {
                    0% { transform: rotate(0deg); }
                    100% { transform: rotate(360deg); }
                }
            </style>
        `;
    }

    // Create error message element
    createErrorElement(message) {
        return `
            <div class="glossary-error" style="background-color: #fee; border: 1px solid #fcc; padding: 15px; border-radius: 5px; color: #c33;">
                <strong>Error loading glossary:</strong> ${message}
                <br><small>Please try refreshing the page.</small>
            </div>
        `;
    }

    // Generate HTML table for glossary
    generateGlossaryHTML(data) {
        if (data.length === 0) {
            return '<p>No glossary entries found.</p>';
        }

        // Sort alphabetically by Spanish word
        const sortedData = [...data].sort((a, b) => 
            a.spanish_word.toLowerCase().localeCompare(b.spanish_word.toLowerCase())
        );

        let html = `
            <div class="glossary-header">
                <p>This glossary contains ${sortedData.length} Spanish words and their English equivalents.</p>
                <p><small><em>Last updated: ${new Date().toLocaleDateString()}</em></small></p>
            </div>
            
            <div class="glossary-search" style="margin-bottom: 20px;">
                <input type="text" id="glossary-search" placeholder="Search glossary..." 
                       style="padding: 10px; border: 1px solid #ddd; border-radius: 4px; width: 100%; max-width: 400px;">
            </div>

            <div class="glossary-table-container" style="overflow-x: auto;">
                <table class="glossary-table" style="width: 100%; border-collapse: collapse; margin-top: 10px;">
                    <thead>
                        <tr style="background-color: #f8f9fa;">
                            <th style="border: 1px solid #ddd; padding: 12px; text-align: left; font-weight: bold;">Spanish Word</th>
                            <th style="border: 1px solid #ddd; padding: 12px; text-align: left; font-weight: bold;">English Equivalent</th>
                            <th style="border: 1px solid #ddd; padding: 12px; text-align: left; font-weight: bold; width: 120px;">Additional Information</th>
                        </tr>
                    </thead>
                    <tbody id="glossary-tbody">
        `;

        sortedData.forEach((entry, index) => {
            const rowStyle = index % 2 === 0 ? 'background-color: #fff;' : 'background-color: #f9f9f9;';
            
            // Check if there's additional info (source or comment)
            const hasAdditionalInfo = entry.source || entry.comment;
            const infoIcon = hasAdditionalInfo ? 
                `<span style="cursor: help; color: #007cba; font-size: 16px;" title="Additional information available">ℹ️</span>` : 
                '';
            
            // Create tooltip content
            let tooltipContent = '';
            if (entry.source) {
                tooltipContent += `<strong>Source:</strong> ${this.escapeHtml(entry.source)}`;
            }
            if (entry.comment) {
                if (tooltipContent) tooltipContent += '<br>';
                tooltipContent += `<strong>Comment:</strong> ${this.escapeHtml(entry.comment)}`;
            }

            html += `
                <tr class="glossary-row" style="${rowStyle}" data-spanish="${entry.spanish_word.toLowerCase()}" data-english="${entry.english_equivalent.toLowerCase()}">
                    <td style="border: 1px solid #ddd; padding: 10px;"><strong>${this.escapeHtml(entry.spanish_word)}</strong></td>
                    <td style="border: 1px solid #ddd; padding: 10px;">${this.escapeHtml(entry.english_equivalent)}</td>
                    <td style="border: 1px solid #ddd; padding: 10px; text-align: center; position: relative;">
                        ${hasAdditionalInfo ? `<span class="info-icon" style="cursor: help; color: #007cba; font-size: 16px; user-select: none;" onmouseover="showTooltip(this, '${tooltipContent.replace(/'/g, '&#39;').replace(/"/g, '&quot;')}')" onmouseout="hideTooltip()" ontouchstart="showTooltip(this, '${tooltipContent.replace(/'/g, '&#39;').replace(/"/g, '&quot;')}')" ontouchend="setTimeout(hideTooltip, 3000)">ℹ️</span>` : ''}
                    </td>
                </tr>
            `;
        });

        html += `
                    </tbody>
                </table>
            </div>
        `;

        return html;
    }

    // Add tooltip functions
    addTooltipFunctions() {
        // Add global tooltip functions if they don't exist
        if (!window.showTooltip) {
            window.showTooltip = function(element, content) {
                // Remove any existing tooltip
                hideTooltip();
                
                // Create tooltip
                const tooltip = document.createElement('div');
                tooltip.id = 'glossary-tooltip';
                tooltip.innerHTML = content;
                tooltip.style.cssText = `
                    position: fixed;
                    background-color: #333;
                    color: white;
                    padding: 10px;
                    border-radius: 6px;
                    font-size: 13px;
                    max-width: 300px;
                    z-index: 10000;
                    box-shadow: 0 4px 8px rgba(0,0,0,0.3);
                    pointer-events: none;
                    line-height: 1.4;
                `;
                
                document.body.appendChild(tooltip);
                
                // Position tooltip near mouse/element
                const rect = element.getBoundingClientRect();
                const tooltipRect = tooltip.getBoundingClientRect();
                
                let left = rect.left + (rect.width / 2) - (tooltipRect.width / 2);
                let top = rect.top - tooltipRect.height - 10;
                
                // Adjust if tooltip goes off screen
                if (left < 10) left = 10;
                if (left + tooltipRect.width > window.innerWidth - 10) {
                    left = window.innerWidth - tooltipRect.width - 10;
                }
                if (top < 10) {
                    top = rect.bottom + 10;
                }
                
                tooltip.style.left = left + 'px';
                tooltip.style.top = top + 'px';
            };
            
            window.hideTooltip = function() {
                const tooltip = document.getElementById('glossary-tooltip');
                if (tooltip) {
                    tooltip.remove();
                }
            };
        }
    }

    // Escape HTML to prevent XSS
    escapeHtml(text) {
        const div = document.createElement('div');
        div.textContent = text;
        return div.innerHTML;
    }

    // Add search functionality
    addSearchFunctionality() {
        const searchInput = document.getElementById('glossary-search');
        if (!searchInput) return;

        searchInput.addEventListener('input', (e) => {
            const searchTerm = e.target.value.toLowerCase();
            const rows = document.querySelectorAll('.glossary-row');

            rows.forEach(row => {
                const spanish = row.getAttribute('data-spanish');
                const english = row.getAttribute('data-english');
                
                if (spanish.includes(searchTerm) || english.includes(searchTerm)) {
                    row.style.display = '';
                } else {
                    row.style.display = 'none';
                }
            });
        });
    }

    // Main method to load and render glossary
    async loadGlossary() {
        const container = document.getElementById(this.containerId);
        if (!container) {
            console.error(`Container with ID '${this.containerId}' not found`);
            return;
        }

        // Show loading state
        container.innerHTML = this.createLoadingElement();

        try {
            // Fetch and parse data
            const csvText = await this.fetchCSVData();
            this.glossaryData = this.parseCSV(csvText);

            // Generate and display glossary
            container.innerHTML = this.generateGlossaryHTML(this.glossaryData);
            
            // Add tooltip functions
            this.addTooltipFunctions();
            
            // Add search functionality
            this.addSearchFunctionality();

            console.log(`Glossary loaded successfully with ${this.glossaryData.length} entries`);

        } catch (error) {
            console.error('Failed to load glossary:', error);
            container.innerHTML = this.createErrorElement(error.message);
        }
    }

    // Method to refresh glossary data
    async refresh() {
        await this.loadGlossary();
    }
}

// Initialize and load glossary when DOM is ready
document.addEventListener('DOMContentLoaded', function() {
    const csvUrl = 'https://docs.google.com/spreadsheets/d/e/2PACX-1vQbZj1biwyJ5sfn-upWjgwu2m4sBm-to4Bxt8WozZ9NwlMN2VirkMxnyyj56X9u_avXzhn98JVmnq4t/pub?output=csv';
    
    // Create glossary instance (change 'glossary-container' to match your div ID)
    window.glossary = new GlossaryGenerator(csvUrl, 'glossary-container');
    
    // Load the glossary
    window.glossary.loadGlossary();
});

// Optional: Add refresh button functionality
function refreshGlossary() {
    if (window.glossary) {
        window.glossary.refresh();
    }
}
</script>
