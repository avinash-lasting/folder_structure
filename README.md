# Update in folder Structure

We are Updating the folder structure for uploads to segregate uploads among companies. Currently uploads of all sibling companies are stores with their parent. Now, Uploads of each company will be stored in their seprate directory.

## New folder structure

Heres the new Structure

```
    - application
    - assets
    - system
    - uploads
        - 1
            - images
            - modules
        - 2
            - images
            - modules
```

Inside uploads each company has its own folder named after its id.

## managing Uploads inside modules

> Now that folder structure is updated, You need to change upload path in your modules so that it points to the correct folder. Change
```
    uploads\modules\modulename\...
```
to
```
    uploads\$companyid\modules\modulename\...
```
Ensure that `$companyid` contains id of currently active company. 

## Updating folder structure

Folder structure is currently Stored in parent db in `meta_data` table with setting_type ='upload_folder_str'. If you need to make any changes in folder structure, update it in the table as well.

> You can either update it mannually or use the function __create_folder_str__ in root folder (lastingerp.com) in home module. Heres a sample of what the structure looks like

```
    private function create_folder_str()
    {
        $folder = [
            'images'    => ['favicon'],
            'modules'   => [
                'account'           => ['pdf_invoice', 'trial_balance','uploads'],
                'amc'               => ['uploads'],
                'auth'              => ['uploads'],
                'bid_management'    => ['uploads'],
                'chat'              => ['uploads'],
                'company'           => ['uploads'],
                'crm'               => ['uploads','downloads'],
                'dashboard'         => ['uploads'],
                'ecommerce'         => ['uploads'],
                'export_import'     => ['uploads'],
                'frontend'          => ['uploads'],
                'home'              => ['uploads'],
                'hrm'               => ['uploads','download','letterPdf'],
                'import'            => ['uploads'],
                'inventory'         => ['uploads' => ['hsn'], 'daily_report', 'reorderFile', 'varient_opt_img'],
                'item'              => ['uploads' => ['cat_images'], 'daily_report', 'reorderFile'],
                'maintenance'       => ['uploads'],
                'npdm'              => ['uploads'],
                'order_management'  => ['uploads'],
                'payment_methods'   => ['uploads'],
                'payroll'           => ['uploads','download','letterPdf'],
                'pms'               => ['uploads'],
                'pos'               => ['uploads'],
                'production'        => ['uploads'],
                'projects'          => ['uploads'],
                'purchase'          => ['uploads','defected_report','pdf','download'],
                'quality_control'   => ['uploads'],
                'recruitment'       => ['uploads','download'],
                'task_management'   => ['uploads','images','letterPdf','download'],
                'users'             => ['uploads'],
                'logistics'         => ['uploads'],
                'integration'       => ['uploads'],
            ]
        ];

        $meta_data = [
            'setting_type' => 'upload_folder_str',
            'setting_value' => json_encode($folder)
        ];

        $folder_structure = $this->db->get_where('meta_data', 'setting_type = "upload_folder_str"')->row();
        if(empty($folder_structure)){
            $this->db->insert('meta_data', $meta_data);
        }else{
            $this->db->where('setting_type = "upload_folder_str"')->update('meta_data', $meta_data);
        }
    }
```

Update the access modifier to public before running the function and revert it back to private to avoid accidental access.

