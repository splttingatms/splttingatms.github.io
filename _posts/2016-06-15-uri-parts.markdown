---
title: URI Parts
layout: post
---

The [System.Uri class](https://msdn.microsoft.com/en-us/library/system.uri.aspx) is .NET's way of representing uniform resource identifiers. The benefit to using Uri objects versus strings is that the Uri class has some properties that make it easy to access parts of the Uri. The table below shows how the Uri class breaks down both web URLs and file paths.

<div class="table-responsive">
	<table class="table table-condensed table-bordered table-striped table-responsive">
		<thead>
			<tr>
				<th>Property</th>
				<th>HTTP URL<br /> <small><code>https://www.sunnyrodriguez.com:80/foo/bar.html?q1=baz&q2=qux</code></small></th>
				<th>File Path <br /> <small><code>C:\Test\Foo\Bar.exe</code></small></th>
			</tr>
		</thead>
		<tbody>
			<tr>
				<th>AbsolutePath</th>
				<td>&quot;/foo/bar.html&quot;</td>
				<td>&quot;C:/Test/Foo/Bar.exe&quot;</td>
			</tr>
			<tr>
				<th>AbsoluteUri</th>
				<td>&quot;https://www.sunnyrodriguez.com:80/foo/bar.html?q1=baz&amp;q2=qux&quot;</td>
				<td>&quot;file:///C:/Test/Foo/Bar.exe&quot;</td>
			</tr>
			<tr>
				<th>LocalPath</th>
				<td>&quot;/foo/bar.html&quot;</td>
				<td>&quot;C:\\Test\\Foo\\Bar.exe&quot;</td>
			</tr>
			<tr>
				<th>Authority</th>
				<td>&quot;www.sunnyrodriguez.com:80&quot;</td>
				<td>&quot;&quot;</td>
			</tr>
			<tr>
				<th>HostNameType</th>
				<td>2</td>
				<td>1</td>
			</tr>
			<tr>
				<th>IsDefaultPort</th>
				<td>false</td>
				<td>true</td>
			</tr>
			<tr>
				<th>IsFile</th>
				<td>false</td>
				<td>true</td>
			</tr>
			<tr>
				<th>IsLoopback</th>
				<td>false</td>
				<td>true</td>
			</tr>
			<tr>
				<th>PathAndQuery</th>
				<td>&quot;/foo/bar.html?q1=baz&amp;q2=qux&quot;</td>
				<td>&quot;C:/Test/Foo/Bar.exe&quot;</td>
			</tr>
			<tr>
				<th>Segments</th>
				<td>[&quot;/&quot;,&quot;foo/&quot;,&quot;bar.html&quot;]</td>
				<td>[&quot;/&quot;,&quot;C:/&quot;,&quot;Test/&quot;,&quot;Foo/&quot;,&quot;Bar.exe&quot;]</td>
			</tr>
			<tr>
				<th>IsUnc</th>
				<td>false</td>
				<td>false</td>
			</tr>
			<tr>
				<th>Host</th>
				<td>&quot;www.sunnyrodriguez.com&quot;</td>
				<td>&quot;&quot;</td>
			</tr>
			<tr>
				<th>Port</th>
				<td>80</td>
				<td>-1</td>
			</tr>
			<tr>
				<th>Query</th>
				<td>&quot;?q1=baz&amp;q2=qux&quot;</td>
				<td>&quot;&quot;</td>
			</tr>
			<tr>
				<th>Fragment</th>
				<td>&quot;&quot;</td>
				<td>&quot;&quot;</td>
			</tr>
			<tr>
				<th>Scheme</th>
				<td>&quot;https&quot;</td>
				<td>&quot;file&quot;</td>
			</tr>
			<tr>
				<th>OriginalString</th>
				<td>&quot;https://www.sunnyrodriguez.com:80/foo/bar.html?q1=baz&amp;q2=qux&quot;</td>
				<td>&quot;C:\\Test\\Foo\\Bar.exe&quot;</td>
			</tr>
			<tr>
				<th>DnsSafeHost</th>
				<td>&quot;www.sunnyrodriguez.com&quot;</td>
				<td>&quot;&quot;</td>
			</tr>
			<tr>
				<th>IdnHost</th>
				<td>&quot;www.sunnyrodriguez.com&quot;</td>
				<td>&quot;&quot;</td>
			</tr>
			<tr>
				<th>IsAbsoluteUri</th>
				<td>true</td>
				<td>true</td>
			</tr>
			<tr>
				<th>UserEscaped</th>
				<td>false</td>
				<td>false</td>
			</tr>
			<tr>
				<th>UserInfo</th>
				<td>&quot;&quot;</td>
				<td>&quot;&quot;</td>
			</tr>
		</tbody>
	</table>
</div>

### Try it yourself
Below is a code snippet that outputs a similar table to console.

<script src="https://gist.github.com/splttingatms/4ba5a0a3e7dbcf837aafc8db1bece555.js"></script>