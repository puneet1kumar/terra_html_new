from parserplan import parse_plan

def analyze_plan(file_path):
    analysis = parse_plan(file_path)
    
    total_resources = analysis['total_resources']
    created_resources = len(analysis['create'])
    read_resources = len(analysis['read'])
    updated_resources = len(analysis['update'])
    deleted_resources = len(analysis['delete'])
    no_op_resources = len(analysis['no-op'])
    delete_create_resources = len(analysis['delete_create'])
    create_delete_resources = len(analysis['create_delete'])
    
    report = {
        'Total Resources': total_resources,
        'Resources to be Created': created_resources,
        'Resources to be Read': read_resources,
        'Resources to be Updated': updated_resources,
        'Resources to be Deleted': deleted_resources,
        'No Operation Resources': no_op_resources,
        'Resources with Delete and Create': delete_create_resources,
        'Resources with Create and Delete': create_delete_resources,
        'Detailed Changes': analysis
    }
    
    return report
