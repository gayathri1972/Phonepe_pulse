.. code:: ipython3

    import csv
    import subprocess
    import pandas as pd
    import requests
    import git
    import pandas as pd
    import os

.. code:: ipython3

    # Clone the Github repository
    os.system("git clone https://github.com/gayathri1972/Phonepe_pulse.git")




.. parsed-literal::

    0



.. code:: ipython3

    import json
    import pandas as pd
    
    root_dir = (r'/Project pulse/pulse-master/data')
    
    # Initialize empty list to hold dictionaries of data for each JSON file
    data_list = []
    
    # Loop over all the state folders
    for state_dir in os.listdir(os.path.join(root_dir, '/Project pulse/pulse-master/data/aggregated/transaction/country/india/state')):
        state_path = os.path.join(root_dir, '/Project pulse/pulse-master/data/aggregated/transaction/country/india/state', state_dir)
        if os.path.isdir(state_path):
    
            # Loop over all the year folders
            for year_dir in os.listdir(state_path):
                year_path = os.path.join(state_path, year_dir)
                if os.path.isdir(year_path):
    
                    # Loop over all the JSON files (one for each quarter)
                    for json_file in os.listdir(year_path):
                        if json_file.endswith('.json'):
                            with open(os.path.join(year_path, json_file)) as f:
                                data = json.load(f)
    
                                # Extract the data we're interested in
                                for transaction_data in data['data']['transactionData']:
                                    row_dict = {
                                        'States': state_dir,
                                        'Transaction_Year': year_dir,
                                        'Quarters': int(json_file.split('.')[0]),
                                        'Transaction_Type': transaction_data['name'],
                                        'Transaction_Count': transaction_data['paymentInstruments'][0]['count'],
                                        'Transaction_Amount': transaction_data['paymentInstruments'][0]['amount']
                                    }
                                    data_list.append(row_dict)
    
    # Convert list of dictionaries to dataframe
    df1 = pd.DataFrame(data_list)
    df1
    




.. raw:: html

    <div>
    <style scoped>
        .dataframe tbody tr th:only-of-type {
            vertical-align: middle;
        }
    
        .dataframe tbody tr th {
            vertical-align: top;
        }
    
        .dataframe thead th {
            text-align: right;
        }
    </style>
    <table border="1" class="dataframe">
      <thead>
        <tr style="text-align: right;">
          <th></th>
          <th>States</th>
          <th>Transaction_Year</th>
          <th>Quarters</th>
          <th>Transaction_Type</th>
          <th>Transaction_Count</th>
          <th>Transaction_Amount</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <th>0</th>
          <td>andaman-&amp;-nicobar-islands</td>
          <td>2018</td>
          <td>1</td>
          <td>Recharge &amp; bill payments</td>
          <td>4200</td>
          <td>1.845307e+06</td>
        </tr>
        <tr>
          <th>1</th>
          <td>andaman-&amp;-nicobar-islands</td>
          <td>2018</td>
          <td>1</td>
          <td>Peer-to-peer payments</td>
          <td>1871</td>
          <td>1.213866e+07</td>
        </tr>
        <tr>
          <th>2</th>
          <td>andaman-&amp;-nicobar-islands</td>
          <td>2018</td>
          <td>1</td>
          <td>Merchant payments</td>
          <td>298</td>
          <td>4.525072e+05</td>
        </tr>
        <tr>
          <th>3</th>
          <td>andaman-&amp;-nicobar-islands</td>
          <td>2018</td>
          <td>1</td>
          <td>Financial Services</td>
          <td>33</td>
          <td>1.060142e+04</td>
        </tr>
        <tr>
          <th>4</th>
          <td>andaman-&amp;-nicobar-islands</td>
          <td>2018</td>
          <td>1</td>
          <td>Others</td>
          <td>256</td>
          <td>1.846899e+05</td>
        </tr>
        <tr>
          <th>...</th>
          <td>...</td>
          <td>...</td>
          <td>...</td>
          <td>...</td>
          <td>...</td>
          <td>...</td>
        </tr>
        <tr>
          <th>3589</th>
          <td>west-bengal</td>
          <td>2022</td>
          <td>4</td>
          <td>Peer-to-peer payments</td>
          <td>184380244</td>
          <td>6.202222e+11</td>
        </tr>
        <tr>
          <th>3590</th>
          <td>west-bengal</td>
          <td>2022</td>
          <td>4</td>
          <td>Merchant payments</td>
          <td>171667404</td>
          <td>1.408077e+11</td>
        </tr>
        <tr>
          <th>3591</th>
          <td>west-bengal</td>
          <td>2022</td>
          <td>4</td>
          <td>Recharge &amp; bill payments</td>
          <td>48921147</td>
          <td>2.602663e+10</td>
        </tr>
        <tr>
          <th>3592</th>
          <td>west-bengal</td>
          <td>2022</td>
          <td>4</td>
          <td>Financial Services</td>
          <td>268388</td>
          <td>2.611229e+08</td>
        </tr>
        <tr>
          <th>3593</th>
          <td>west-bengal</td>
          <td>2022</td>
          <td>4</td>
          <td>Others</td>
          <td>610414</td>
          <td>4.579379e+08</td>
        </tr>
      </tbody>
    </table>
    <p>3594 rows × 6 columns</p>
    </div>



.. code:: ipython3

    import os
    import json
    import pandas as pd
    
    root_dir = 'c:/Project pulse/pulse-master/data/aggregated/user/country/india'
    df2_list = []
    
    for state_dir in os.listdir(root_dir):
        state_path = os.path.join(root_dir, state_dir)
        if os.path.isdir(state_path):
            for json_file in os.listdir(state_path):
                if json_file.endswith('.json'):
                    with open(os.path.join(state_path, json_file), 'r') as f:
                        json_data = json.load(f)
                        if isinstance(json_data, list):
                            df2_list += json_data
                        else:
                            df2_list.append(json_data)
            if df2_list:
                df2 = pd.json_normalize(df2_list)
                df2['subfolder'] = state_dir
                df2['subsubfolder'] = 'state'
    df2 = pd.DataFrame(data_list)

.. code:: ipython3

    df2




.. raw:: html

    <div>
    <style scoped>
        .dataframe tbody tr th:only-of-type {
            vertical-align: middle;
        }
    
        .dataframe tbody tr th {
            vertical-align: top;
        }
    
        .dataframe thead th {
            text-align: right;
        }
    </style>
    <table border="1" class="dataframe">
      <thead>
        <tr style="text-align: right;">
          <th></th>
          <th>States</th>
          <th>Transaction_Year</th>
          <th>Quarters</th>
          <th>Transaction_Type</th>
          <th>Transaction_Count</th>
          <th>Transaction_Amount</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <th>0</th>
          <td>andaman-&amp;-nicobar-islands</td>
          <td>2018</td>
          <td>1</td>
          <td>Recharge &amp; bill payments</td>
          <td>4200</td>
          <td>1.845307e+06</td>
        </tr>
        <tr>
          <th>1</th>
          <td>andaman-&amp;-nicobar-islands</td>
          <td>2018</td>
          <td>1</td>
          <td>Peer-to-peer payments</td>
          <td>1871</td>
          <td>1.213866e+07</td>
        </tr>
        <tr>
          <th>2</th>
          <td>andaman-&amp;-nicobar-islands</td>
          <td>2018</td>
          <td>1</td>
          <td>Merchant payments</td>
          <td>298</td>
          <td>4.525072e+05</td>
        </tr>
        <tr>
          <th>3</th>
          <td>andaman-&amp;-nicobar-islands</td>
          <td>2018</td>
          <td>1</td>
          <td>Financial Services</td>
          <td>33</td>
          <td>1.060142e+04</td>
        </tr>
        <tr>
          <th>4</th>
          <td>andaman-&amp;-nicobar-islands</td>
          <td>2018</td>
          <td>1</td>
          <td>Others</td>
          <td>256</td>
          <td>1.846899e+05</td>
        </tr>
        <tr>
          <th>...</th>
          <td>...</td>
          <td>...</td>
          <td>...</td>
          <td>...</td>
          <td>...</td>
          <td>...</td>
        </tr>
        <tr>
          <th>3589</th>
          <td>west-bengal</td>
          <td>2022</td>
          <td>4</td>
          <td>Peer-to-peer payments</td>
          <td>184380244</td>
          <td>6.202222e+11</td>
        </tr>
        <tr>
          <th>3590</th>
          <td>west-bengal</td>
          <td>2022</td>
          <td>4</td>
          <td>Merchant payments</td>
          <td>171667404</td>
          <td>1.408077e+11</td>
        </tr>
        <tr>
          <th>3591</th>
          <td>west-bengal</td>
          <td>2022</td>
          <td>4</td>
          <td>Recharge &amp; bill payments</td>
          <td>48921147</td>
          <td>2.602663e+10</td>
        </tr>
        <tr>
          <th>3592</th>
          <td>west-bengal</td>
          <td>2022</td>
          <td>4</td>
          <td>Financial Services</td>
          <td>268388</td>
          <td>2.611229e+08</td>
        </tr>
        <tr>
          <th>3593</th>
          <td>west-bengal</td>
          <td>2022</td>
          <td>4</td>
          <td>Others</td>
          <td>610414</td>
          <td>4.579379e+08</td>
        </tr>
      </tbody>
    </table>
    <p>3594 rows × 6 columns</p>
    </div>



.. code:: ipython3

    import os
    import json
    import pandas as pd
    
    root_dir = (r'/content/pulse/data')
    
    # Initialize empty list to hold dictionaries of data for each JSON file
    data_list = []
    
    # Loop over all the state folders
    for state_dir in os.listdir(os.path.join(root_dir, 'C:/Project pulse/pulse-master/data/map/transaction/hover/country/india/state')):
        state_path = os.path.join(root_dir, 'C:/Project pulse/pulse-master/data/map/transaction/hover/country/india/state', state_dir)
        if os.path.isdir(state_path):
    
            # Loop over all the year folders
            for year_dir in os.listdir(state_path):
                year_path = os.path.join(state_path, year_dir)
                if os.path.isdir(year_path):
    
                    # Loop over all the JSON files (one for each quarter)
                    for json_file in os.listdir(year_path):
                        if json_file.endswith('.json'):
                            with open(os.path.join(year_path, json_file)) as f:
                                data = json.load(f)
    
                                # Extract the data we're interested in
                                for hoverDataList in data['data']['hoverDataList']:
                                    row_dict = {
                                        'States': state_dir,
                                        'Transaction_Year': year_dir,
                                        'Quarters': int(json_file.split('.')[0]),
                                        'District': hoverDataList['name'],
                                        'Transaction_Type': hoverDataList['metric'][0]['type'],
                                        'Transaction_Count': hoverDataList['metric'][0]['amount']
                                    }
                                    data_list.append(row_dict)
    
    # Convert list of dictionaries to dataframe
    df3 = pd.DataFrame(data_list)
    
    df3
    




.. raw:: html

    <div>
    <style scoped>
        .dataframe tbody tr th:only-of-type {
            vertical-align: middle;
        }
    
        .dataframe tbody tr th {
            vertical-align: top;
        }
    
        .dataframe thead th {
            text-align: right;
        }
    </style>
    <table border="1" class="dataframe">
      <thead>
        <tr style="text-align: right;">
          <th></th>
          <th>States</th>
          <th>Transaction_Year</th>
          <th>Quarters</th>
          <th>District</th>
          <th>Transaction_Type</th>
          <th>Transaction_Count</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <th>0</th>
          <td>andaman-&amp;-nicobar-islands</td>
          <td>2018</td>
          <td>1</td>
          <td>north and middle andaman district</td>
          <td>TOTAL</td>
          <td>9.316631e+05</td>
        </tr>
        <tr>
          <th>1</th>
          <td>andaman-&amp;-nicobar-islands</td>
          <td>2018</td>
          <td>1</td>
          <td>south andaman district</td>
          <td>TOTAL</td>
          <td>1.256025e+07</td>
        </tr>
        <tr>
          <th>2</th>
          <td>andaman-&amp;-nicobar-islands</td>
          <td>2018</td>
          <td>1</td>
          <td>nicobars district</td>
          <td>TOTAL</td>
          <td>1.139849e+06</td>
        </tr>
        <tr>
          <th>3</th>
          <td>andaman-&amp;-nicobar-islands</td>
          <td>2018</td>
          <td>2</td>
          <td>north and middle andaman district</td>
          <td>TOTAL</td>
          <td>1.317863e+06</td>
        </tr>
        <tr>
          <th>4</th>
          <td>andaman-&amp;-nicobar-islands</td>
          <td>2018</td>
          <td>2</td>
          <td>south andaman district</td>
          <td>TOTAL</td>
          <td>2.394824e+07</td>
        </tr>
        <tr>
          <th>...</th>
          <td>...</td>
          <td>...</td>
          <td>...</td>
          <td>...</td>
          <td>...</td>
          <td>...</td>
        </tr>
        <tr>
          <th>14631</th>
          <td>west-bengal</td>
          <td>2022</td>
          <td>4</td>
          <td>nadia district</td>
          <td>TOTAL</td>
          <td>2.804568e+10</td>
        </tr>
        <tr>
          <th>14632</th>
          <td>west-bengal</td>
          <td>2022</td>
          <td>4</td>
          <td>birbhum district</td>
          <td>TOTAL</td>
          <td>1.614650e+10</td>
        </tr>
        <tr>
          <th>14633</th>
          <td>west-bengal</td>
          <td>2022</td>
          <td>4</td>
          <td>purba medinipur district</td>
          <td>TOTAL</td>
          <td>3.309949e+10</td>
        </tr>
        <tr>
          <th>14634</th>
          <td>west-bengal</td>
          <td>2022</td>
          <td>4</td>
          <td>maldah district</td>
          <td>TOTAL</td>
          <td>2.721861e+10</td>
        </tr>
        <tr>
          <th>14635</th>
          <td>west-bengal</td>
          <td>2022</td>
          <td>4</td>
          <td>darjiling district</td>
          <td>TOTAL</td>
          <td>1.801650e+10</td>
        </tr>
      </tbody>
    </table>
    <p>14636 rows × 6 columns</p>
    </div>



.. code:: ipython3

    import os
    import json
    import pandas as pd
    
    root_dir = 'C:/Project pulse/pulse-master/data/map/user/hover/country/india/state'
    
    # Initialize empty list to hold dictionaries of data for each JSON file
    data_list = []
    
    # Loop over all the state folders
    for state_dir in os.listdir(root_dir):
        state_path = os.path.join(root_dir, state_dir)
        if os.path.isdir(state_path):
    
            # Loop over all the year folders
            for year_dir in os.listdir(state_path):
                year_path = os.path.join(state_path, year_dir)
                if os.path.isdir(year_path):
    
                    # Loop over all the JSON files (one for each quarter)
                    for json_file in os.listdir(year_path):
                        if json_file.endswith('.json'):
                            with open(os.path.join(year_path, json_file)) as f:
                                data = json.load(f)
    
                                # Extract the data we're interested in
                                for district, values in data['data']['hoverData'].items():
                                    row_dict = {
                                        'States': state_dir,
                                        'Transaction_Year': year_dir,
                                        'Quarter': int(json_file.split('.')[0]),
                                        'District': district,
                                        'RegisteredUsers': values['registeredUsers'],
                                    }
                                    data_list.append(row_dict)
    
    # Convert list of dictionaries to dataframe
    df4 = pd.DataFrame(data_list)
    
    df4




.. raw:: html

    <div>
    <style scoped>
        .dataframe tbody tr th:only-of-type {
            vertical-align: middle;
        }
    
        .dataframe tbody tr th {
            vertical-align: top;
        }
    
        .dataframe thead th {
            text-align: right;
        }
    </style>
    <table border="1" class="dataframe">
      <thead>
        <tr style="text-align: right;">
          <th></th>
          <th>States</th>
          <th>Transaction_Year</th>
          <th>Quarter</th>
          <th>District</th>
          <th>RegisteredUsers</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <th>0</th>
          <td>andaman-&amp;-nicobar-islands</td>
          <td>2018</td>
          <td>1</td>
          <td>north and middle andaman district</td>
          <td>632</td>
        </tr>
        <tr>
          <th>1</th>
          <td>andaman-&amp;-nicobar-islands</td>
          <td>2018</td>
          <td>1</td>
          <td>south andaman district</td>
          <td>5846</td>
        </tr>
        <tr>
          <th>2</th>
          <td>andaman-&amp;-nicobar-islands</td>
          <td>2018</td>
          <td>1</td>
          <td>nicobars district</td>
          <td>262</td>
        </tr>
        <tr>
          <th>3</th>
          <td>andaman-&amp;-nicobar-islands</td>
          <td>2018</td>
          <td>2</td>
          <td>north and middle andaman district</td>
          <td>911</td>
        </tr>
        <tr>
          <th>4</th>
          <td>andaman-&amp;-nicobar-islands</td>
          <td>2018</td>
          <td>2</td>
          <td>south andaman district</td>
          <td>8143</td>
        </tr>
        <tr>
          <th>...</th>
          <td>...</td>
          <td>...</td>
          <td>...</td>
          <td>...</td>
          <td>...</td>
        </tr>
        <tr>
          <th>14635</th>
          <td>west-bengal</td>
          <td>2022</td>
          <td>4</td>
          <td>nadia district</td>
          <td>1359420</td>
        </tr>
        <tr>
          <th>14636</th>
          <td>west-bengal</td>
          <td>2022</td>
          <td>4</td>
          <td>birbhum district</td>
          <td>855236</td>
        </tr>
        <tr>
          <th>14637</th>
          <td>west-bengal</td>
          <td>2022</td>
          <td>4</td>
          <td>purba medinipur district</td>
          <td>1346908</td>
        </tr>
        <tr>
          <th>14638</th>
          <td>west-bengal</td>
          <td>2022</td>
          <td>4</td>
          <td>maldah district</td>
          <td>954892</td>
        </tr>
        <tr>
          <th>14639</th>
          <td>west-bengal</td>
          <td>2022</td>
          <td>4</td>
          <td>darjiling district</td>
          <td>564562</td>
        </tr>
      </tbody>
    </table>
    <p>14640 rows × 5 columns</p>
    </div>



.. code:: ipython3

    import os
    import json
    import pandas as pd
    
    root_dir = (r'C:/Project pulse/pulse-master/data')
    
    # Initialize empty list to hold dictionaries of data for each JSON file
    data_list = []
    
    # Loop over all the state folders
    for state_dir in os.listdir(os.path.join(root_dir, 'C:/Project pulse/pulse-master/data/top/transaction/country/india/state')):
        state_path = os.path.join(root_dir, 'C:/Project pulse/pulse-master/data/top/transaction/country/india/state', state_dir)
        if os.path.isdir(state_path):
    
            # Loop over all the year folders
            for year_dir in os.listdir(state_path):
                year_path = os.path.join(state_path, year_dir)
                if os.path.isdir(year_path):
    
                    # Loop over all the JSON files (one for each quarter)
                    for json_file in os.listdir(year_path):
                        if json_file.endswith('.json'):
                            with open(os.path.join(year_path, json_file)) as f:
                                data = json.load(f)
    
                                # Extract the data we're interested in
                                for districts in data['data']['districts']:
                                    row_dict = {
                                        'States': state_dir,
                                        'Transaction_Year': year_dir,
                                        'Quarters': int(json_file.split('.')[0]),
                                        'District': districts['entityName'],
                                        'Transaction_Type': districts['metric']['type'],
                                        'Transaction_Count': districts['metric']['count'],
                                        'Transaction_Amount': districts['metric']['amount']
                                    }
                                    data_list.append(row_dict)
    
    # Convert list of dictionaries to dataframe
    df5 = pd.DataFrame(data_list)
    
    df5




.. raw:: html

    <div>
    <style scoped>
        .dataframe tbody tr th:only-of-type {
            vertical-align: middle;
        }
    
        .dataframe tbody tr th {
            vertical-align: top;
        }
    
        .dataframe thead th {
            text-align: right;
        }
    </style>
    <table border="1" class="dataframe">
      <thead>
        <tr style="text-align: right;">
          <th></th>
          <th>States</th>
          <th>Transaction_Year</th>
          <th>Quarters</th>
          <th>District</th>
          <th>Transaction_Type</th>
          <th>Transaction_Count</th>
          <th>Transaction_Amount</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <th>0</th>
          <td>andaman-&amp;-nicobar-islands</td>
          <td>2018</td>
          <td>1</td>
          <td>south andaman</td>
          <td>TOTAL</td>
          <td>5688</td>
          <td>1.256025e+07</td>
        </tr>
        <tr>
          <th>1</th>
          <td>andaman-&amp;-nicobar-islands</td>
          <td>2018</td>
          <td>1</td>
          <td>nicobars</td>
          <td>TOTAL</td>
          <td>528</td>
          <td>1.139849e+06</td>
        </tr>
        <tr>
          <th>2</th>
          <td>andaman-&amp;-nicobar-islands</td>
          <td>2018</td>
          <td>1</td>
          <td>north and middle andaman</td>
          <td>TOTAL</td>
          <td>442</td>
          <td>9.316631e+05</td>
        </tr>
        <tr>
          <th>3</th>
          <td>andaman-&amp;-nicobar-islands</td>
          <td>2018</td>
          <td>2</td>
          <td>south andaman</td>
          <td>TOTAL</td>
          <td>9395</td>
          <td>2.394824e+07</td>
        </tr>
        <tr>
          <th>4</th>
          <td>andaman-&amp;-nicobar-islands</td>
          <td>2018</td>
          <td>2</td>
          <td>nicobars</td>
          <td>TOTAL</td>
          <td>1120</td>
          <td>3.072437e+06</td>
        </tr>
        <tr>
          <th>...</th>
          <td>...</td>
          <td>...</td>
          <td>...</td>
          <td>...</td>
          <td>...</td>
          <td>...</td>
          <td>...</td>
        </tr>
        <tr>
          <th>5915</th>
          <td>west-bengal</td>
          <td>2022</td>
          <td>4</td>
          <td>south twenty four parganas</td>
          <td>TOTAL</td>
          <td>15651650</td>
          <td>3.373698e+10</td>
        </tr>
        <tr>
          <th>5916</th>
          <td>west-bengal</td>
          <td>2022</td>
          <td>4</td>
          <td>purba medinipur</td>
          <td>TOTAL</td>
          <td>14484229</td>
          <td>3.309949e+10</td>
        </tr>
        <tr>
          <th>5917</th>
          <td>west-bengal</td>
          <td>2022</td>
          <td>4</td>
          <td>hooghly</td>
          <td>TOTAL</td>
          <td>13931352</td>
          <td>2.755409e+10</td>
        </tr>
        <tr>
          <th>5918</th>
          <td>west-bengal</td>
          <td>2022</td>
          <td>4</td>
          <td>howrah</td>
          <td>TOTAL</td>
          <td>13350090</td>
          <td>2.793786e+10</td>
        </tr>
        <tr>
          <th>5919</th>
          <td>west-bengal</td>
          <td>2022</td>
          <td>4</td>
          <td>paschim medinipur</td>
          <td>TOTAL</td>
          <td>12768161</td>
          <td>2.521681e+10</td>
        </tr>
      </tbody>
    </table>
    <p>5920 rows × 7 columns</p>
    </div>



.. code:: ipython3

    import os
    import json
    import pandas as pd
    
    root_dir = 'C:/Project pulse/pulse-master/data/top/user/country/india/state'
    
    # Initialize empty list to hold dictionaries of data for each JSON file
    data_list = []
    
    # Loop over all the state folders
    for state_dir in os.listdir(root_dir):
        state_path = os.path.join(root_dir, state_dir)
        if os.path.isdir(state_path):
    
            # Loop over all the year folders
            for year_dir in os.listdir(state_path):
                year_path = os.path.join(state_path, year_dir)
                if os.path.isdir(year_path):
    
                    # Loop over all the JSON files
                    for json_file in os.listdir(year_path):
                        if json_file.endswith('.json'):
                            with open(os.path.join(year_path, json_file)) as f:
                                data = json.load(f)
    
                                # Extract the data we're interested in
                                for district in data['data']['districts']:
                                    row_dict = {
                                        'State': state_dir,
                                        'Transaction_Year': year_dir,
                                        'Quarters': int(json_file.split('.')[0]),
                                        'District': district['name'] if 'name' in district else district['pincode'],
                                        'RegisteredUsers': district['registeredUsers'],
                                    }
                                    data_list.append(row_dict)
    
    # Convert list of dictionaries to dataframe
    df6 = pd.DataFrame(data_list)
    df6




.. raw:: html

    <div>
    <style scoped>
        .dataframe tbody tr th:only-of-type {
            vertical-align: middle;
        }
    
        .dataframe tbody tr th {
            vertical-align: top;
        }
    
        .dataframe thead th {
            text-align: right;
        }
    </style>
    <table border="1" class="dataframe">
      <thead>
        <tr style="text-align: right;">
          <th></th>
          <th>State</th>
          <th>Transaction_Year</th>
          <th>Quarters</th>
          <th>District</th>
          <th>RegisteredUsers</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <th>0</th>
          <td>andaman-&amp;-nicobar-islands</td>
          <td>2018</td>
          <td>1</td>
          <td>south andaman</td>
          <td>5846</td>
        </tr>
        <tr>
          <th>1</th>
          <td>andaman-&amp;-nicobar-islands</td>
          <td>2018</td>
          <td>1</td>
          <td>north and middle andaman</td>
          <td>632</td>
        </tr>
        <tr>
          <th>2</th>
          <td>andaman-&amp;-nicobar-islands</td>
          <td>2018</td>
          <td>1</td>
          <td>nicobars</td>
          <td>262</td>
        </tr>
        <tr>
          <th>3</th>
          <td>andaman-&amp;-nicobar-islands</td>
          <td>2018</td>
          <td>2</td>
          <td>south andaman</td>
          <td>8143</td>
        </tr>
        <tr>
          <th>4</th>
          <td>andaman-&amp;-nicobar-islands</td>
          <td>2018</td>
          <td>2</td>
          <td>north and middle andaman</td>
          <td>911</td>
        </tr>
        <tr>
          <th>...</th>
          <td>...</td>
          <td>...</td>
          <td>...</td>
          <td>...</td>
          <td>...</td>
        </tr>
        <tr>
          <th>5915</th>
          <td>west-bengal</td>
          <td>2022</td>
          <td>4</td>
          <td>howrah</td>
          <td>1422011</td>
        </tr>
        <tr>
          <th>5916</th>
          <td>west-bengal</td>
          <td>2022</td>
          <td>4</td>
          <td>nadia</td>
          <td>1359420</td>
        </tr>
        <tr>
          <th>5917</th>
          <td>west-bengal</td>
          <td>2022</td>
          <td>4</td>
          <td>purba medinipur</td>
          <td>1346908</td>
        </tr>
        <tr>
          <th>5918</th>
          <td>west-bengal</td>
          <td>2022</td>
          <td>4</td>
          <td>paschim medinipur</td>
          <td>1217113</td>
        </tr>
        <tr>
          <th>5919</th>
          <td>west-bengal</td>
          <td>2022</td>
          <td>4</td>
          <td>purba bardhaman</td>
          <td>1119310</td>
        </tr>
      </tbody>
    </table>
    <p>5920 rows × 5 columns</p>
    </div>



.. code:: ipython3

    # Data transformation on file1
    # Drop any duplicates
    d1 = df1.drop_duplicates()
    d2 = df2.drop_duplicates()
    d3 = df3.drop_duplicates()
    d4 = df4.drop_duplicates()
    d5 = df5.drop_duplicates()
    d6 = df6.drop_duplicates()

.. code:: ipython3

    #checking Null values
    null_counts = d1.isnull().sum()
    print(null_counts)


.. parsed-literal::

    States                0
    Transaction_Year      0
    Quarters              0
    Transaction_Type      0
    Transaction_Count     0
    Transaction_Amount    0
    dtype: int64
    

.. code:: ipython3

    null_counts = d2.isnull().sum()
    print(null_counts)


.. parsed-literal::

    States                0
    Transaction_Year      0
    Quarters              0
    Transaction_Type      0
    Transaction_Count     0
    Transaction_Amount    0
    dtype: int64
    

.. code:: ipython3

    null_counts = d3.isnull().sum()
    print(null_counts)


.. parsed-literal::

    States               0
    Transaction_Year     0
    Quarters             0
    District             0
    Transaction_Type     0
    Transaction_Count    0
    dtype: int64
    

.. code:: ipython3

    null_counts = d4.isnull().sum()
    print(null_counts)


.. parsed-literal::

    States              0
    Transaction_Year    0
    Quarter             0
    District            0
    RegisteredUsers     0
    dtype: int64
    

.. code:: ipython3

    null_counts = d5.isnull().sum()
    print(null_counts)


.. parsed-literal::

    States                0
    Transaction_Year      0
    Quarters              0
    District              0
    Transaction_Type      0
    Transaction_Count     0
    Transaction_Amount    0
    dtype: int64
    

.. code:: ipython3

    null_counts = d6.isnull().sum()
    print(null_counts)


.. parsed-literal::

    State               0
    Transaction_Year    0
    Quarters            0
    District            0
    RegisteredUsers     0
    dtype: int64
    

.. code:: ipython3

    #converting all dataframes in to csv
    d1.to_csv('agg_trans.csv', index=False)
    df2.to_csv('agg_user.csv', index=False)
    d3.to_csv('map_tran.csv', index=False)
    d4.to_csv('map_user.csv', index=False)
    d5.to_csv('top_tran.csv', index=False)
    d6.to_csv('top_user.csv', index=False)

.. code:: ipython3

    Agg_trans = pd.read_csv(r'/content/agg_trans.csv')
    
