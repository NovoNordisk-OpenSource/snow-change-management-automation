# ServiceNow Create Change Action

This GitHub Action creates an IT change request in ServiceNow.

## Inputs

| Parameter | Required | Description |
|-----------|----------|-------------|
| `instance` | Yes | Your ServiceNow instance URL (e.g., `your-instance.service-now.com`) |
| `username` | Yes | ServiceNow username with appropriate permissions |
| `password` | Yes | ServiceNow password or API key |
| `short_description` | Yes | Short description of the change |
| `description` | Yes | Detailed description of the change |
| `impact` | No | Impact level (1=High, 2=Medium, 3=Low). Default: `3` |
| `urgency` | No | Urgency level (1=High, 2=Medium, 3=Low). Default: `3` |
| `assignment_group` | Yes | Name of the assignment group |

## Outputs

| Output | Description |
|--------|-------------|
| `change_number` | The number of the created change |
| `sys_id` | The sys_id of the created change |
| `link` | URL to the created change |

## Example Usage

```yaml
name: Create ServiceNow Change

on:
  workflow_dispatch:
    inputs:
      description:
        description: 'Change description'
        required: true

jobs:
  create-change:
    runs-on: ubuntu-latest
    steps:
      - name: Create ServiceNow Change
        id: create_change
        uses: ./.github/actions/snow-create-change
        with:
          instance: 'your-instance.service-now.com'
          username: ${{ secrets.SNOW_USERNAME }}
          password: ${{ secrets.SNOW_PASSWORD }}
          short_description: 'Deploy new application version'
          description: ${{ github.event.inputs.description }}
          impact: '3'
          urgency: '3'
          assignment_group: 'IT Operations'
    
      - name: Display Change Details
        run: |
          echo "Change Number: ${{ steps.create_change.outputs.change_number }}"
          echo "Change Link: ${{ steps.create_change.outputs.link }}"
```

## Setting Up Secrets

1. Go to your GitHub repository
2. Navigate to Settings > Secrets and variables > Actions
3. Click "New repository secret"
4. Add the following secrets:
   - `SNOW_USERNAME`: Your ServiceNow username
   - `SNOW_PASSWORD`: Your ServiceNow password or API key

## Development

1. Install dependencies:
   ```bash
   npm install
   ```

2. Build the action:
   ```bash
   npm run build
   ```

## License

This project is licensed under the MIT License.
