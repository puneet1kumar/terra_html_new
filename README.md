import json

def parse_plan(file_path):
    with open(file_path, 'r') as file:
        data = json.load(file)

    resources = data.get('resource_changes', [])
    analysis = {
        'no-op': [],
        'create': [],
        'read': [],
        'update': [],
        'delete': [],
        'delete_create': [],  # This is for ["delete", "create"]
        'create_delete': [],  # This is for ["create", "delete"]
        'total_resources': len(resources)
    }

    for resource in resources:
        change = resource['change']['actions']
        resource_info = {
            'address': resource['address'],
            'type': resource['type'],
            'name': resource['name'],
            'provider': resource['provider_name'],
            'values': resource['change']['after']
        }

        if change == ["no-op"]:
            analysis['no-op'].append(resource_info)
        elif change == ["create"]:
            analysis['create'].append(resource_info)
        elif change == ["read"]:
            analysis['read'].append(resource_info)
        elif change == ["update"]:
            analysis['update'].append(resource_info)
        elif change == ["delete"]:
            analysis['delete'].append(resource_info)
        elif change == ["delete", "create"]:
            analysis['delete_create'].append(resource_info)
        elif change == ["create", "delete"]:
            analysis['create_delete'].append(resource_info)

    return analysis
