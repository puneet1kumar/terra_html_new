<!doctype html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Analysis Report</title>
    <!-- Bootstrap CSS -->
    <link href="https://maxcdn.bootstrapcdn.com/bootstrap/4.5.2/css/bootstrap.min.css" rel="stylesheet">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/5.15.4/css/all.min.css">
    <style>
        body {
            font-family: Arial, sans-serif;
            background-color: #f0c6b7;
        }
        .container {
            background: #fff;
            padding: 15px;
            border-radius: 15px;
            box-shadow: 0 0 20px rgba(188, 8, 8, 0.1);
            margin-top: 25px;
        }
        .summary p {
            font-size: 18px;
            margin-bottom: 15px;
        }
        .create {
            color: #28a745;
        }
        .read {
            color: #17a2b8;
        }
        .update {
            color: #ffc107;
        }
        .delete {
            color: #dc3545;
        }
        .no-op {
            color: #6c757d;
        }
        .delete_create {
            color: #e83e8c;
        }
        .create_delete {
            color: #fd7e14;
        }
        .resources {
            display: none;
            animation: fadeIn 0.5s;
        }
        .toggle-btn {
            cursor: pointer;
            color: #040c16;
        }
        .toggle-btn:hover {
            text-decoration: underline;
        }
        .card-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
        }
        .mb-4 {
            align-items: center;
            color: #040b64;
            box-shadow: 0 0 30px rgba(188, 8, 8, 0.1);
            background: #fff;
            padding: 5px;
            border-radius: 5px;
        }
        .mb-3 {
            color: #040b64;
            box-shadow: 0 0 30px rgba(188, 8, 8, 0.1);
            background: #fff;
            padding: 5px;
            border-radius: 5px;
        }
        .resource-details {
            display: none;
            margin-top: 10px;
        }
        .resource-address {
            cursor: pointer;
            font-weight: bold;
            color: #007bff;
        }
        .resource-address:hover {
            text-decoration: underline;
        }
        .values {
            margin-left: 20px;
            background: #f7f7f7;
            padding: 10px;
            border-radius: 5px;
            box-shadow: 0 0 10px rgba(0, 0, 0, 0.05);
        }
        .values dt {
            font-weight: bold;
        }
        .values dd {
            margin-bottom: 10px;
        }
        @keyframes fadeIn {
            from { opacity: 0; }
            to { opacity: 1; }
        }
    </style>
</head>
<body>
    <div class="container mt-5">
        <h1 class="text-center mb-4">Terraform Plan Analysis Report</h1>
        <div class="summary mb-4">
            <p>Total Resources: <strong>{{ report['Total Resources'] }}</strong></p>
            <p>Resources to be Created: <span class="create">{{ report['Resources to be Created'] }}</span></p>
            <p>Resources to be Read: <span class="read">{{ report['Resources to be Read'] }}</span></p>
            <p>Resources to be Updated: <span class="update">{{ report['Resources to be Updated'] }}</span></p>
            <p>Resources to be Deleted: <span class="delete">{{ report['Resources to be Deleted'] }}</span></p>
            <p>No Operation Resources: <span class="no-op">{{ report['No Operation Resources'] }}</span></p>
            <p>Resources with Delete and Create: <span class="delete_create">{{ report['Resources with Delete and Create'] }}</span></p>
            <p>Resources with Create and Delete: <span class="create_delete">{{ report['Resources with Create and Delete'] }}</span></p>
        </div>

        <div class="details">
            <h2 class="mb-4">Detailed Changes</h2>
            {% for change_type, resources in report['Detailed Changes'].items() %}
                {% if change_type != 'total_resources' %}
                    <div class="card mb-3">
                        <div class="card-header">
                            <h3 class="m-0">{{ change_type.replace('_', ' ').capitalize() }}: {{ resources|length }}</h3>
                            <button class="btn btn-sm btn-info toggle-btn" onclick="toggleResources('{{ change_type }}')">
                                <i class="fas fa-chevron-down"></i> <b>Show/Hide</b>
                            </button>
                        </div>
                        <ul class="resources list-group list-group-flush" id="{{ change_type }}">
                            {% for resource in resources %}
                                <li class="list-group-item {{ change_type }}">
                                    <span class="resource-address" onclick="toggleDetails('{{ resource['address']|replace('.', '_') }}')">
                                        <i class="fas fa-{{ 'plus-circle' if change_type == 'create' else 'book' if change_type == 'read' else 'edit' if change_type == 'update' else 'trash-alt' if change_type == 'delete' else 'sync' if change_type == 'delete_create' else 'exchange-alt' if change_type == 'create_delete' else 'minus-circle' }}"></i>
                                        {{ resource['address'] }}
                                    </span>
                                    <div class="resource-details" id="{{ resource['address']|replace('.', '_') }}">
                                        <strong>Type:</strong> {{ resource['type'] }}<br>
                                        <strong>Name:</strong> {{ resource['name'] }}<br>
                                        <strong>Provider:</strong> {{ resource['provider_name'] }}<br>
                                        <strong>Values:</strong>
                                        <dl class="values">
                                            {% for key, value in resource['change'].get('after', {}).items() %}
                                                <dt>{{ key }}</dt>
                                                <dd>{{ value }}</dd>
                                            {% endfor %}
                                        </dl>
                                    </div>
                                </li>
                            {% endfor %}
                        </ul>
                    </div>
                {% endif %}
            {% endfor %}
        </div>
    </div>

    <script>
        function toggleResources(changeType) {
            var resourcesList = document.getElementById(changeType);
            if (resourcesList.style.display === "none" || resourcesList.style.display === "") {
                resourcesList.style.display = "block";
            } else {
                resourcesList.style.display = "none";
            }
        }

        function toggleDetails(addressId) {
            var details = document.getElementById(addressId);
            if (details.style.display === "none" || details.style.display === "") {
                details.style.display = "block";
            } else {
                details.style.display = "none";
            }
        }
    </script>
</body>
</html>
