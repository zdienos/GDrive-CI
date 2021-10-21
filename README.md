# GDrive-CI


public function gfile()
    {
        $response = array();
        $gfile    = new Gdrive();
        $service = $gfile->g_drive();
        $optParams = array(
            'pageSize'  => 5,
            'fields' => "nextPageToken, files(contentHints/thumbnail,fileExtension,iconLink,id,name,size,thumbnailLink,webContentLink,webViewLink,mimeType,parents)",
        );
        $result = $service->files->listFiles($optParams);
        if (count($result->getFiles()) == 0) {
            $response['success'] = false;
            $response['messages'] = 'Fili Google Drive tidak ditemukan';
        } else {
            $data['data'] = array();
            $result->getFiles();
            $no = 1;
            foreach ($result as $key => $value) {
                $ops = '<div class="btn-group">';
                                $ops .= '<a type="button" class="btn btn-sm btn-info" href="' . $value->getwebViewLink() . '" target="__blank"><i class="fa fa-download"> Download</i></a>';
                $ops .= '</div>';
                $data['data'][$key] = array(
                    $no++,
                    $value->getName(),
                    $value->getId(),
                    $value->getwebViewLink(),
                    $ops
                );
            }
        }
        return $this->response->setJSON($data);
    }

<script>
var base_url ='<?=base_url()';
    $(function() {
        $('#data_table').DataTable({
            "paging": true,
            "lengthChange": true,
            "searching": true,
            "ordering": true,
            "info": true,
            "autoWidth": false,
            "responsive": true,
            "ajax": {
                "url": base_url + 'gfile',
                "type": "GET",
                "dataType": "json",
                async: "true"
            }
        });
    });
</script>
