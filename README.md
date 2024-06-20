import json
from jinja2 import Environment, FileSystemLoader
from analyzer import analyze_plan

# Load and analyze the plan.json file
report = analyze_plan('plan.json')

# Set up Jinja2 environment and load template
env = Environment(loader=FileSystemLoader('.'))
template = env.get_template('template.html')

# Render the template with data from the analysis
output = template.render(report=report)

# Write the rendered HTML to report.html
with open('report.html', 'w') as file:
    file.write(output)

print("Report generated: report.html")
